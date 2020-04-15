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

Assuming you already obtained a token 

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

### Tools

* [peirates](https://github.com/inguardians/peirates) - a Kubernetes penetration tool, enables an attacker to escalate privilege and pivot through a Kubernetes cluster. It automates known techniques to steal and collect service accounts, obtain further code execution, and gain control of the cluster.

* [kube-hunter](https://github.com/aquasecurity/kube-hunter) - hunts for security weaknesses in Kubernetes clusters.

* [kubeaudit](https://github.com/Shopify/kubeaudit) - is a command line tool to audit Kubernetes clusters for various different security concerns: run the container as a non-root user, use a read only root filesystem, drop scary capabilities, don't add new ones, don't run privileged etc.

* [kube-bench](https://github.com/aquasecurity/kube-bench) - Checks whether Kubernetes is deployed according to security best practices as defined in the CIS Kubernetes Benchmark.

* [katacoda](https://katacoda.com/) - Learn Kubernetes using interactive broser-based scenarios.


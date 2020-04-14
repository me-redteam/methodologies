# Kubernetes Penetration Testing

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

Main deployments options:
1. On-premise 
2. Managed Cloud 

Note: Security controls and attack surface will differ from deployment to deployment. 

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

//Add Nmap Line


External Assessment

Internal Assessment 

Compromised Container Assessment

Post-authentication

Once you've gained root access to a master or a worker node you can perform an authenticated CIS Kubernetes compliance scan using Nessus. 

Compromise the image registry and upload backdoored image clones. 

### Tools

[kubeaudit](https://github.com/Shopify/kubeaudit) - is a command line tool to audit Kubernetes clusters for various different security concerns: run the container as a non-root user, use a read only root filesystem, drop scary capabilities, don't add new ones, don't run privileged etc.


kube-bench 

https://katacoda.com/

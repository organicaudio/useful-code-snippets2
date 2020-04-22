# Kuberentes

Kubernetes (K8s) allows to deploy multiple containers on one or multiple host systems.

## terms

- kube-scheduler
    - assign pods (container[s]) to nodes (hosts)
    - quality of service
    - policies
    - user specification
- one master node and multiple worker nodes
- allows rolling upgrade and a rollback
- secret management defined via namespaces


Master node:
- API server
- web ui (dashboard)
- Scheduler
- Controller
- etcd DB

Worker node:
- kubelet (kubernetes node agent)
- Docker (or containerd compliant software)
- kube-proxy (supported modes: user space, iptables, ipvs)

Pods:
- are one or multipel containers which
- share a unique network IP Address


controllers allow:
1. fail over
2. scaling
3. load balancing

Kind of controllers:
- RepilcaSet: ensures that a specified number of pods is runnning in a node (needs another controller as wrapper)
- Deployment: declarative describe pods and ReplicaSets
- DaemonsSets: ensures that all nodes run a specific copy of a pod (1 DaemonSet - 1 Pod - X nodes).
- Jobs: supervisor for pods which do batch jobs
- Services: allow communication between deployment controllers

Services:
- internal: pods can only communicate within k8s cluster over there IP
- external: pods can be accessed through the node ip and the so called NodePort
- load balancer: to expose application to the internet

Labels:
- added to objects like pods, services, deployments
- key-value pairs to identify attributes of objects

Selectors:
- allows to select objects by a label
- equality-based: `=` and `!=`
- set-based: `IN`, `NOTIN`, `EXISTS`
  
Namespaces:
- allows multiple virtual clusters on the same physical host
- there is a default namescpace

## install

- Install Docker like in the [official documentation](https://docs.docker.com/engine/install/) or use the install script `curl -sSL https://get.docker.com | sh` which might be easier
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- install [auto completion for kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enable-kubectl-autocompletion): `kubectl completion bash >/etc/bash_completion.d/kubectl` *might get permission issues with this command. Than write it  to ~/kubectl and mv it with sudo to the target folder*
- if you want to test stuff on your local machine  [install minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/#before-you-begin). **Note the 'Before you begin' section which shows you how to check for a supervisor.** If you do not have one install one like VirtualBox.

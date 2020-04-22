# Kuberentes 

Kubernetes (K8s) allows to deploy multiple containers on one or multiple host systems. The [offial documentation of k8s](https://kubernetes.io/docs/) is outstanding. There is also a [interactive tutorial]([offcial tutorial](https://kubernetes.io/docs/tutorials/kubernetes-basics/))

outline of features:
- one master node and multiple worker nodes
- creates self-healing clusters
- allows rolling upgrade and rollback
- secret management and encapsulation via namespaces

## terms

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

base objects:
- pod - to run container
- service - ro access pods
- volume - to persists data from pods
- namespace - to separate objects 

Pods:
- are one or multiple containers which
- share a unique network IP Address
- "Containers should only be scheduled together in a single Pod if they are tightly coupled and need to share resources such as disk."

controllers allow:
1. fail over
2. scaling
3. load balancing

Kind of controllers:
- RepilcaSet: ensures that a specified number of pods is runnning in a node (needs another controller as wrapper like deployment)
- Deployment: declarative describe how to create and update instances (self-healing mechanism)
- DaemonsSets: ensures that all nodes run a specific copy of a pod (1 DaemonSet - 1 Pod - X nodes).
- Jobs: supervisor for pods which do batch jobs
- Services: allow communication between deployment controllers

Services:
- internal: pods can only communicate within k8s cluster over there IP (default behaviour of pods without a service)
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

## kubectl cli

[kubectl reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

basic commands:
- `kubectl cluster-info`
- `kubectl create -f FILENAME` create objects and controllers from a yaml file
- `kubectl get all`
    - `kubectl get nodes`
    - `kubectl get services`
    - `kubectl get deployments`
    - `kubectl get rs`

create resources without k8s file:
- `kubectl run <pod_name> --image=<image_name>` starts a pod with the given image
- `kubectl create deployment <deployment_name/pod_name> --image=<image_name>` creates a deployment which starts a pod with the given image
- `kubectl expose <pod_name> --type=NodePort --port=80` creates a service on port 80 (of the node/host machine)

debug pod:
- `kubectl proxy ` proxy into a cluster even when no services are exposed. Pods are accessible via there pod name or there internal ip address. Terminate with ctrl + c.
- `kubectl describe <node/pods/deployment>`
- `kubectl kubectl logs <pod_name>`
- `kubectl exec <pod_name>` - execute a command on a container in a pod
- `kubectl exec -ti <pod_name> bash` open bash in container
## minicube cli

- `minicube start`
- `minicube service <pod_name>` shows the service in the browser

## k8s yaml file

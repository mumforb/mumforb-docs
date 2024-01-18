Docker compose (vs dockerfile):



# Kubernetes
System for automating deployment, scalping and management of containerized applications
It’s like docker compose, but with scaling and health and everything else
Like a conductor of a container orchestra, so to speak
Load balancing, service discovering
Storage orchestration
Automate rollouts/rollbacks
Self-healing
Secret and config management
Horizontally scale

Moves from current to desired state (adding containers, what have you)


## Pod
The packaging/box for a container. A container sits in a pod
(Spacesuit, with a container as the person inside)

### Deploy pods

### Services
Resources you can use to check on pods

### Nodes
Master node has an etcd store, which has everything needed to track what’s happening inside it
Also a controller manager and a scheduler
Also an api server
Kubectl connects to the master
Master communicates with the various nodes

### Kubelet
Software running on the node that allows communication with the master

### Why Kubernetes
Assuming containers: accelerate dev onboarding, columns, eliminate app conflicts, environment consistency

Orchestrate containers, zero-downtime deployments, self-healing powers, scale containers … and more

### Why Kubernetes for a developer
Emulate production locally. Move from Docker Compose to K8s. Create an e2e testing env. Ensure app scales properly. Ensure secrets/config are working properly. Performance testing. Workload scenarios (ci/cd, etc). Learn how to leverage deployment options. Help devops cerate resources and solve problems.

### Options for running K8s locally
Minikube
Docker desktop

### Run in docker desktop

Simple as enabling k8s inside docker desktop. Windows or Mac OS. 

### Using kubectl

- kubectl version 
- kubecl cluster-info
- kubectl get all
kubectl run [container-name] --image=[image-name]
kubectl port-forward [pod][ports]
kubectl expose ...
bubectl create [resource]
bubectl apply [resource]

alias k="kubectl"

### K8s Web UI Dashboard

Enable:
- kubectl apply [dashboard-yaml-url]
- kubectl apply -f dashboard.adminuser.yml
- kubectl create token admin-user -n kube-system
- kubectl proxy
- visit the dashboard url and login


## Links

- https://github.com/kubernetes/examples # all kinds of official examples of how to do things - a source of truth
- https://www.pluralsight.com/courses/kubernetes-developers-core-concepts # many of these docs came from watching this course
- https://github.com/DanWahlin/DockerAndKubernetesCourseCode # GitHub repo with all the examples and such for this course


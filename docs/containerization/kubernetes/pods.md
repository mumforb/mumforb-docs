# Pods

The basic execute unit of a K8s application. Pods run containers, one or theoretically more than one. Think of pods as the way to organize parts into pods - normally a single process per container, and a single container per pod.

Has:
ip address
memory
volumens
etc.

Pods live and die but never come back to life

Master scheduled one or more pods in a node. Pods will have a unique Ip address. Pod containers shar ethe same network namespace. Container processes need t bind to different ports within a pod. (A sidecar container would need its own port if it's at the same internal IP address.)

Pods NEVER span nodes.

## Creating Pods

kubectl run command is an easy, but non-declarative way (imperative) to do it. Kubectl create/apply works with yaml files. 

Ports are only acessible within a cluster by default. you can expose ports externally via kubectl port-forward `kubectl port-forward [name-of-pod] 8080:80` where 8080 is extenral and 80 is internal. This is the most basic way to accomlish this.

## Deleting Pods

The big idea here is that you can delete a pod but if you don't delete the deployment it'll just come back again.

## Common commands

- kubectl run my-nginx --image=nginx:alpine (THIS IS JUST THE NAME OF A DOCKER IMAGE - SO IT'S LOOKING AT DOCKER'S REGISTRY) 
- kubectl delete 
- kubectl get all 
- kubectl port-forward my-nginx 8080:80
- kubectl delete pod [pod-name]

## Health

Probes are diagnistic performed by the kubelet on a container. 

Liveness: is the pod sick or healthy?
Readiness: is it ready to receive requests?

Example of a liveness probe:

```
apiVersion: v1
kind: Pod
...
spec:
    containers:
    - name: my-nginx
      image: nginx:alpine
      livenessProbe:
        httpGet:
            path: /index.html
            port: 80
        initialDelaySeconds: 15
        timeoutSeconds: 2
        periodSeconds: 5
        failureThreshold: 1
```



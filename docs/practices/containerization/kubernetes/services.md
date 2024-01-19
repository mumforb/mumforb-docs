# Services

Services talk inside the pod, and from outside the cluster.

A single point of entry to access one o rmore pod. Simple example: if an IP changes, a service tracks that change. 

- Abstract pod IP addresses from consumers
- Load balance between pods
- Relies on labels to associate a service with a pod
- Node's kube-proxy creates a virtual IP for services
- Utilizes layer 4 (TCP/UDP over IP)
- Creates endpoints that sit between the sercice and the pods

So you might have a front-end service that talks to ssveral load balanced pods, and then a backend service that only talks to one.

It appears that services actually RUN on the NODE itself, rather than on a pod (or as a separate pod).

## Types

ClusterIP
Expose the service on a clusetr-internal IP (default)
The service itself is only used withint the cluster. Pods can talk to other pods via this ClusterIP service.

NodePort
Expose the sercice on each ode's IP at a static port.
This allocates a port frome a range by default. Each node proxies the allocated port. So this allows an external caller to proxy into a pod (or pods). Useful for debugging or somethign else.

LoadBalancer
Provision an external IP to act as a load balancer for the service.
Exposes a service externally. Particularly useful when combined with a cloud providers load balancer.

ExternalName
Maps a service to the DNS name
The service just acts as an alias for an external service. So the service just proxies over to the external service. It's a sync from outside into inside, and the external service details are hidden from the cluster.

## kubectl

Using kubectl to access the insdie of a cluster via a service.

kubectl port-forward pod/pod-name 8080:80
kubectl port-forward deploymeny/deploymnet-name 8080:80
kubectl port-forward service/service-name 8080:80

## With YAML

A simple example.

```
apiVersion: v1
kind: Service
metadata:
    name: frontend # each service automatically gets an ip address - so there will be something available at frontend:80
    labels:
        app: my-nginx
spec:
    type: ClusterIP # this is the default - can be NodePort, LoadBalancer, or ExternalName with different sub-options
    selector:
        app: nginx
    ports:
        name: http
        port: 80
        targetPort: 80
```

## Deploy yaml with kubectl

- kubectl create -f file.service.yml
- kubectl apply -f file.service.yml 

As always create/apply do a lot of the same thing, and there are valid reasons to use either/both.

## Test to see if a service

Sneak right into a pod, then see if that pod can talk to other pods.

- kubectl exec (pod-name) -- curl -s http://podIP

## In action

Get a pod's IP

kubectl get pod name-of-pod -o # -o means yml

Copy ip address. Then curl from point a to point b.

OR

Use the service's IP itself. So from a pod, you can hit the service's IP and it should take you over to the correct pod. You can also use the internal dns name like my-nginx:8080.

Slightly different techniques for each type of service, but the same idea is relevant.


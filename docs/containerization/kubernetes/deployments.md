# Deployments

A ReplicaSet is a declarative way to manage pods. A deployment is even higher than that, it's a declarative way to manage pods using a ReplicaSet. 

Deployments (via ReplicaSets) take care of ensuring pods stay running (or get replaced). A ReplicaSet sits outside a pod. it's responsible for healing (which is really a way to say resurrecting). Provides fault-tolerance, scaling, ensuring requested number of pods are available. ReplicaSets are used by deployments.

So Deployments utilize ReplicaSets to create Pods which contain Containers. Deployments give us zero-downtime updates, as well as rollbacks, autoscaling, and create unique labels.

```
apiVersion: v1
kind: Deployment
metadata:
    name: frontend
    labels:
        app: my-nginx
        tier: frontend
spec:
    selector:
        matchLabels:
            tier: frontend
    template:
        metadata:
            labels:
                tier: frontend
        spec:
            containers:
                - name: my-nginx
                  image: nginx:alpine
```

To actually run it: 
kubectl apply -f file.deployment.yml (or create, but usually apply)

kubectl get deployments
kubectl get deployment --show-labels - used to organize things
kubectl get deployment -l app-nginx - find just the things with that label
kubectl delete deployment [deployment-name]

these two do the same thing mostly 
kubectl scale deployment [deployment-name] --replicas=5
kubectl scale deployment -f file.deployment.yml  --replicas=5

Can also add replicas right in the yml file itself.

## Constraints

you can limit the resources used by a deployment in the spec. 

kubectl descrieb deployment [name-of-deployment]
kubectl describe deployments
kubectl get deployments -l app=my-nginx
kubectl scale -f filename.yml --replicas=4

Zero downtime deployments allow software updates to be deployed to production without impacting end users. This is a huge benefit of K8s.

Options:
- rolling - kind of like the default
- blue/green - split between old and new
- canary - a small amount of traffic goes to one pod
- rollbacks - oops

## Rolling

New pods more or less replace older pods one at a time until only new pods are left. This is the automatic method if you do `kubectl apply -f file.deployment.yml`. 


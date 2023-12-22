# Rollouts

Since the term deployment is already used to apply to something that is smaller than a complete release of all the assets of your application, I'm using the term rollout to refer to an orchestrated release of multiple resources (potentially including deployments, but also secrets and configmaps and everything else).

Steps might be:

- create secrets using kubectl
- kubectl apply -f .k8s # where k8s contains a number of yml files of all flavors


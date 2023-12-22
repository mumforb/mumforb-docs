# K8s yml

this is a viable way, but isn't necessarily the default. Good to know because it's a good fundamental.

This is identical to using kubectl to do this manually.

```
apiVersion: v1
kind: Pod
metadata:
    name: my-nginx
spec:
    containers: 
    - name: my-nginx
    image: nginx:alpine
```

`kubectl create -f file.pod.yml --dry-run --validate=true`
`kubectl create -f file.pod.yml`
`kubectl apply -f file.pod.yml` - this works if it exists or if it doesn't; create will give you an error if it already exists, while apply will make updates. It also creates if it hasn't alreadty been created.
`kubectl create -f -file.pod.yml --save-config` - use --save-config when you want to use kubectl apply in the future

This talks to the master node and processes the request. Save config causes the reousr'ces confi settings to be saved in annotations. It actually saves it in the metadata of the yml. I think.

You can delete then via the yml file that was used to create, just by using "delete" instead of "create".


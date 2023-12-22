# Secrets

An object that contains a small amount of sensitive data.

Kubernetes has away of storing sensitive info. Can mount as env variables or files. Only makes secrets available to nodes that have a pod requesting the secret. And secrets are stored in tmpfs on a node (not on disk).

Definitely want to limit access to etcd where secrets are stored. RBAC should be used to create pods because that's a way to back into secrets.

## Creating

kubectl create secret generic my-secret --from-literal=pwd=my-password
kubectl create secret generic my-secret --from-file=ssh-privatekey=~/.ssh/rsa_pub

Via yaml? Yes, but it's only base64 encoded!

```
kind: Secret
metadata:
    name: db-passwords
type: Opaque
data:
    app-password: Base64etc=
    admin-password: Base64etc=
```

## Accessing

kubectl get secrets
kubectl get secrets db-passwords -o yaml


```
apiVersion: v1
...
spec:
    template:
    ...
    containers:
    env:
    - name: DATABASE_PASSWORD
      valueFrom:
        configMapKeyRef:
            name: db-password
            key: db-password
```

... and so on, in the same way you get values out of configmaps.


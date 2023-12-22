# Storage

If a container/pod goes down, how do you keep your data?

A storage volume exists "alongside" your pods.

## Volumes

Not the only option, but it answers the question how do you store data and state for pods and containters, and also for sharing purposes. Logs and data for a database, the containers rely on a mountPath to access the volume. Each pod can have one or more volume attached.

A volue refernec ea a storage location. Must have a unique name. Attached to a pod and may or not be tied to the lifetime.

They generally get created declaratively via yaml, so there aren't any kubectl examples here so far.

An example of creating an emptyDir type of volume in a simple pod. It launches an alpine pod that updates the index.html at the shared location with the current time every 10 seconds. 

A yaml example:

```
apiVersion: v1
kind: PersistentVolume
metadata:
    name: my-pv
spec:
    capacity: 10Gi
    accessModes:
        - ReadWriteOnce
        - ReadOnlyMany
    persistentVolumeReclaimPolicy: Retain
    azureFile:
        secretName: <azure-secret>
        shareName: <name_from_azure>
        readOnly: false
```

And a corresponding PVC:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: pv-dd-account-hdd-5g
    annotations:
        volume.beta.kubernetes.io/storage-class: accounthdd
spec:
    accessModes:
        - ReadWriteOnce
    resources: 
        requests:
            storage: 5Gi 
```

And in a corresponding Pod:

```
apiVersion: v1
kind: Pod
metadata:
    name: pod-uses-account-hdd-5g
    labels:
        name: storage
spec:
    containers:
        - image: nginx
          name: az-c-01
          command:
          - /bin/sh
          - -c
          - while true; do echo $(date) >> /mnt/blobdisk/outfile; sleep 1; done
volumeMounts:
- name: blobdisk01
  mountPath: /mnt/blobdisk
volumes:
- name: blobdisk01
  persistentVolumeClaim:
    claimName: pv-dd-account-hdd-5g
```

kubectl describe pod [pod-name] will show any volumes, as will viewing the yaml.

### emptyDir

mepty dir, it's transiet. Lives and dies along with the containing pod. 

### hostPath

pod mounts into the nodes filesystem. So for example, it might mount to the Docker socket. Pros: easy to set up. Cons: what if the worker goes down. There are a lot of options within this hostPath structure. So this means Docker is available (and shared, pretty much) within the pod that has this volume shared. 

### nfs

mounts to a share, typically provided by a cloud service. Super handy for this kind of thing, because the storage gets all the magic of a cloud provider's storage solutions, but is still available via your cluster.

## PersistentVolumes

A cluster-wide storage unit provisioned by an administrator with a lifecycle independent from a pod. Maybe to a cloud storage solution, maybe to a NAS. It's abstracted away. PVCs (PersistentVolumeClaims) are how you connect to it.

A yaml example:


```
apiVersion: v1
kind: Pod
metadata:
    name: my-nginx
spec:
    volumes:
        - name: html
          emptyDir: {} 
    containers:
        - name: nginx
          image: nginx:alpine
          volumeMounts:
            - name: html
              mountPath: /usr/share/nginx/html # this is nginx's default serving path
              readOnly: true
        - name: html-updater
          image: alpine
          command: ["/bin/sh", "-c"]
          args:
            - while true; do date >> /html/index.html; sleep 10; done
          volumeMounts:
            - name: html
              mountPath: /html # doesn't matter so much where this is mounted since it's just doing an update
```

### persistentVolumeClaim

The connection flow, technically, goes like this: Pod > PVC > PV > STORAGE


## StorageClasses

Type of storage template that can e used to dynamically provision storage. Used to define different "classes" of storage. Acts as a type of storage template. Supports dynamic provisioning of PersistentVolumes. Admins dont' have to create PVs ahead of time if it's set up this way.

Create a PVC that references the SC, which sort of sits between the PVC and the PV - but the PVC connects directly back to the PVC. 

Yaml for a local storage PersistentVolume


```
apiVersion: v1
kind: PersistentVolume
metadata:
    name: my-pv
spec:
    capacity: 10Gi
    volumeMode: Block
    accessModes:
        - ReadWriteOnce
    storageClassName: local-storage
    local:
        path: /data/storage
    nodeAffinity:
        required:
            nodeSelectorTerm:
            - matchExpressions:
              - key: kubernetes.io/hostname
                operator: In
                values:
                - <node-name>
```

and the corresponding claim

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: my-pvc
spec:
    accessModes:
    - ReadWriteOnce
    storageClassName: local-storage
    resources: 
        requests:
            storage: 1Gi 
```

And the corresponding pod:

```
apiVersion: v1
kind: Pod
...
spec:
    volumes:
    - name: my-volume
      persistentVolumeClaim:
        claimName: my-pvc
```


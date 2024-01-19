# ConfigMaps

Closely related to secrets. It's a way to store config information and provide it to containers. Used to inject config data in to a container. Can store entire files or provide key/value pairsL

Filename becomes the key. Also can put them on the command-line. Also a yaml file as a "manifest".

Access those values via env vars, or as a ConfigMap Volume (which you access as a file).

## Creating a ConfigMap

A yaml example of some data registered inside a cluster via a ConfigMap.

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-settings
  labels:
    app: app-settings
data:
  enemies: aliens
  lives: "3"
  enemies.cheat: "true"
  enemies.cheat.level: noGoodRotten
  ```

  kubectl create -f file.configmap.yml

  OR

  Just a recular file:

  ```
enemies=aliens
lives=3
enemies.cheat=true
enemies.cheat.level=noGoodRotten
```
kubectl create configmap [cm-name] --from-file=[path-to-file]


Becomes this yaml:


```
apiVersion: v1
kind: ConfigMap
data:
    game.config: |-
        enemies=aliens
        lives=3
        enemies.cheat=true
        enemies.cheat.level=noGoodRotten
```
OR

Can also use tha tregular file to create env variables:


kubectl create configmap [cm-name] --from-env-file=[path-to-file]

Becomes this yaml:

```
apiVersion: v1
kind: ConfigMap
data:
    enemies=aliens
    lives=3
    enemies.cheat=true
    enemies.cheat.level=noGoodRotten
```

OR

Can also load it straight via kubectl with the `--from-literal=thing` flag.

## Using A ConfigMap

kubectl get cm [name] -o yaml

Can load as an env via yaml that looks like this:

```
apiVersion: v1
...
spec:
    template:
    ...
    containers:
    env:
    - name: ENEMIES
      valueFrom:
        configMapKeyRef:
            name: app-settings
            key: enemies
```

OR

```
apiVersion: v1
...
spec:
    template:
    ...
    containers:
        envFrom:
        - configMapRef:
          name: app-settings
```

OR

"VOLUME"

```
apiVersion: v1
...
spec:
    template:
    ...
    volumes:
        - name: app-config-vol
          configMap:
            name: app-settings
    containers:
        volumeMounts:
            - name: app-config-vol
              mountPath: /etc/config
```

## uses

Displaying a value from a configMap all the way over into the front-end via a Docker image which is being served in a cluster. 

## viewing

kubectl get cm -name-of-cm -o


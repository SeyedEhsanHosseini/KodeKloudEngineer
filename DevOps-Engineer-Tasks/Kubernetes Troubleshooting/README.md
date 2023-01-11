## We can list all of the pods, services, stateful sets, and other resources by using the kubectl get all command.
```
thor@jump_host ~$ kubectl get all
```

#### Output:
```
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4h58m
```
## list all files in current directory
```
thor@jump_host ~$ ls -l
```
#### Output:
```
total 4
-rw-r--r-- 1 thor thor 2338 Dec  7 20:54 mysql_deployment.yml
```
## Try to create Kubernetes resources directly using kubectl -f command
```
thor@jump_host ~$ kubectl create -f mysql_deployment.yml 
```
#### Output:
```
unable to recognize "mysql_deployment.yml": no matches for kind "PersistentVolume" in version "apps/v1"
unable to recognize "mysql_deployment.yml": no matches for kind "PersistentVolumeClaim" in version "apps/v1"
error validating "mysql_deployment.yml": error validating data: ValidationError(Service.spec): unknown field "tier" in io.k8s.api.core.v1.ServiceSpec; if you choose to ignore these errors, turn validation off with --validate=false
```

## Check mysql_deployment.yml to find syntax errors
```
thor@jump_host ~$ vi mysql_deployment.yml
```
#### Output:
```
apiVersion: apps/v1
kind: PersistentVolume
metadata:
  name: mysql-pv
  labels:
  type: local
spec:
  storageClassName: standard
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
  path: "/mnt/data"
  persistentVolumeReclaimPolicy:
  -  Retain
---
apiVersion: apps/v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
  labels:
  app: mysql-app
spec:
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250MB
---
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql-app
spec:
  type: NodePort
  ports:
    - targetPort: 3306
      port: 3306
      nodePort: 30011

  selector:
    app: mysql-app
  tier: mysql
---
apiVersion: v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql-app
spec:
  selector:
    matchLabels:
      app: mysql-app
    tier: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql-app
      tier: mysql
    spec:
      containers:
      - images: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
          secretKeyRef:
            name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
          secretKeyRef:
            name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:

              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
          persistentVolumeClaim:
          claimName: mysql-pv-claim

```
## Create a copy of manifest to fix syntax errors
```          
thor@jump_host ~$ cp mysql_deployment.yml mysql_deployment_troubleshoot.yml 
```
## The new manifest must be like this
```
thor@jump_host ~$ vi mysql_deployment_troubleshoot.yml 
```
#### Output:
```
---
apiVersion: v1 
kind: PersistentVolume 
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: standard
  capacity:
    storage: 250Mi
  accessModes: 
     -  ReadWriteOnce 
  hostPath:
    path: "/mnt/data"
  persistentVolumeReclaimPolicy: Retain
---
apiVersion: v1 
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
  labels:
    type: mysql-app 
spec:
  storageClassName: standard
  accessModes: 
  - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi 

---
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql-app
spec:
  type: NodePort
  ports:
    - targetPort: 3306
      port: 3306
      nodePort: 30011
  selector:
    app: mysql_app
    tier: mysql

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql-app
spec:
  selector:
    matchLabels:
      app: mysql-app
      tier: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql-app
        tier: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
           claimName: mysql-pv-claim

```
## Check diffrences between manifests using diff command
```
thor@jump_host ~$ diff mysql_deployment.yml mysql_deployment_troubleshoot.yml 
```
#### Output:
```
1,2c1,3
< apiVersion: apps/v1 
---
> ---
> apiVersion: v1 

########################################

5,6c6,7
<   labels: 
<   type: local 
---
>   labels:
>     type: local

########################################

12c13
<     - ReadWriteOnce
---
>      -  ReadWriteOnce 

########################################

14,19c15,20
<   path: "/mnt/data" 
<   persistentVolumeReclaimPolicy:
<   -  Retain
< ---
< apiVersion: apps/v1 
---
>     path: "/mnt/data"
>   persistentVolumeReclaimPolicy: Retain
> 
> ---
> apiVersion: v1 

########################################

23,24c24,25
<   app: mysql-app 
< spec:
---
>     type: mysql-app 
> spec:

########################################

26,27c27,28
<   accessModes:
<     - ReadWriteOnce
---
>   accessModes: 
>   - ReadWriteOnce

########################################

29,30c30,32
<     requests: 
<       storage: 250MB 
---
>     requests:
>       storage: 250Mi 
> 

########################################

44,46c46,49
<   selector:
<     app: mysql-app
<   tier: mysql
---
>   selector:
>     app: mysql_app
>     tier: mysql
> 

########################################

48,49c51,52
< apiVersion: v1 
---
> apiVersion: apps/v1

########################################

58c61
<     tier: mysql 
---
>       tier: mysql

########################################

65,68c68,71
<       tier: mysql 
<     spec:

---
>         tier: mysql
>     spec:


########################################

70,75c73,78
<         env:
<         - name: MYSQL_ROOT_PASSWORD 
<           valueFrom:
<           secretKeyRef: 
<             name: mysql-root-pass 
<               key: password 
---
>         env:
>         - name: MYSQL_ROOT_PASSWORD
>           valueFrom:
>             secretKeyRef:
>               name: mysql-root-pass
>               key: password

########################################

78,80c81,83
<           secretKeyRef: 
<             name: mysql-db-url 
<               key: database 
---
>             secretKeyRef:
>               name: mysql-db-url
>               key: database

########################################

```
## Overwrite contents of old manifest with new manifest using cat command
```
thor@jump_host ~$ cat mysql_deployment_troubleshoot.yml > mysql_deployment.yml 
```
## Try againg to create Kubernetes resources directly using kubectl -f command
```
thor@jump_host ~$ kubectl create -f mysql_deployment.yml 
```
#### Output:
```
persistentvolume/mysql-pv created
persistentvolumeclaim/mysql-pv-claim created
service/mysql created
deployment.apps/mysql-deployment created
```
## List all resources to validate the task
```
thor@jump_host ~$ kubectl get all
```
#### Output:
```
NAME                                    READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-74f5dd5cdf-qfd95   1/1     Running   0          39s
```
```
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP          4h12m
service/mysql        NodePort    10.96.6.5    <none>        3306:30011/TCP   39s
```
```
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           39s
```
```
NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-74f5dd5cdf   1         1         1       39s
```

## We can list all of the pods, services, stateful sets, and other resources by using the kubectl get all command.
```
thor@jump_host ~$ kubectl get all
```
#### Output:
```
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   44m
```

## Create a manifest nginx-replicationcontroller.yml and add the following lines to it

```
thor@jump_host ~$ vi /tmp/nginx-replicationcontroller 
```

```
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-replicationcontroller
  labels:
    app: nginx_app
    type: front-end
spec:
  replicas: 3
  selector:
    app: nginx_app
  template:
    metadata:
      name: nginx_pod
      labels:
        app: nginx_app
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
```
## Try to create Kubernetes resources directly using kubectl -f command
```
thor@jump_host ~$ kubectl create -f /tmp/nginx-replicationcontroller 
```
#### Output:
```
replicationcontroller/nginx-replicationcontroller created
```
## List all resources to validate the task
```
thor@jump_host ~$ kubectl get all
```
#### Output:
```
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-replicationcontroller-8nlvm   1/1     Running   0          16s
pod/nginx-replicationcontroller-l9t56   1/1     Running   0          16s
pod/nginx-replicationcontroller-s9sh7   1/1     Running   0          16s
```
```
NAME                                                DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx-replicationcontroller   3         3         3       16s
```
```
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   56m
```
## Run the following command to create a namespace :

```
thor@jump_host ~$ kubectl create namespace httpd-namespace-datacenter
```
#### Output:
```
namespace/httpd-namespace-datacenter created
```
## List all namespaces :
```
thor@jump_host ~$ kubectl get namespaces
```
#### Output:
```
NAME                         STATUS   AGE
default                      Active   51m
httpd-namespace-datacenter   Active   13s
kube-node-lease              Active   51m
kube-public                  Active   51m
kube-system                  Active   51m
local-path-storage           Active   50m
```

## Create a yaml file named httpd.yml and add the following lines to it then save & exit :
```
thor@jump_host ~$ vi /tmp/httpd.yml
```

```
---
apiVersion: v1
kind: Service
metadata:
  name: httpd-service-datacenter
  namespace: httpd-namespace-datacenter
spec:
  type: NodePort
  selector:
    app: httpd-app-datacenter
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30004
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deployment-datacenter
  namespace: httpd-namespace-datacenter
  labels:
    app: httpd-app-datacenter
spec:
  replicas: 2
  selector:
    matchLabels:
      app: httpd-app-datacenter
  template:
    metadata:
      labels:
        app: httpd-app-datacenter
    spec:
      containers:
        - name: httpd-container-datacenter
          image: httpd:latest
          ports:
            - containerPort: 80
```

## create resource(s) from httpd.yml file

```       
thor@jump_host ~$ kubectl create -f /tmp/httpd.yml 
```
#### Output:
```
service/httpd-service-datacenter created
deployment.apps/httpd-deployment-datacenter created
```





## Create jenkins namespace
```‍‍
thor@jump_host ~$ kubectl create namespace jenkins
```
#### Output
```
namespace/jenkins created
```
## List all namespaces
```
thor@jump_host ~$ kubectl get namespaces
```
#### Output:
```
NAME                 STATUS   AGE
default              Active   50m
jenkins              Active   11s
kube-node-lease      Active   50m
kube-public          Active   50m
kube-system          Active   50m
local-path-storage   Active   46m
```
## Create jenkins.yml file and add following lines to it
```
thor@jump_host ~$ vi /tmp/jenkins.yml
```

```
apiVersion: v1

kind: Service

metadata:

  name: jenkins-service

  namespace: jenkins

spec:

  type: NodePort

  selector:

    app: jenkins

  ports:

    - port: 8080

      targetPort: 8080

      nodePort: 30008

---

apiVersion: apps/v1

kind: Deployment

metadata:

  name: jenkins-deployment

  namespace: jenkins

  labels:

    app: jenkins

spec:

  replicas: 1

  selector:

    matchLabels:

      app: jenkins

  template:

    metadata:

      labels:

        app: jenkins

    spec:

      containers:

        - name: jenkins-container

          image: jenkins/jenkins

          ports:

            - containerPort: 8080
```

## Create resources using "kubectl create" command 

```
thor@jump_host ~$ kubectl create -f /tmp/jenkins.yml
```

##  List all resources in jenkins namespace
```
thor@jump_host ~$ kubectl get all -n jenkins
```
#### Output:
```
NAME                                      READY   STATUS    RESTARTS   AGE
pod/jenkins-deployment-6b6c78f968-gn42x   1/1     Running   0          71s

NAME                      TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
service/jenkins-service   NodePort   10.96.19.47   <none>        8080:30008/TCP   73s

NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/jenkins-deployment   1/1     1            1           73s

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/jenkins-deployment-6b6c78f968   1         1         1       72s
```




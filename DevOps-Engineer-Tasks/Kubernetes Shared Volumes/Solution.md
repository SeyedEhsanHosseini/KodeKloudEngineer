## Ensure kubectl works properly on jump_host
```
thor@jump_host ~$ kubectl get services
```
#### Output:
```
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4h4m
```
```
thor@jump_host ~$ kubectl get pods
```
#### Output:
```
No resources found in default namespace.
```
## The following lines should be written into a YML file, then saved and exited.
```
thor@jump_host ~$ vi /tmp/volume.yml
```

```
apiVersion: v1
kind: Pod
metadata: 
  name: volume-share-nautilus
  labels: 
    name: my-app
spec: 
  volumes:
    - name: volume-share
      emptyDir: {}
  containers:
    - name: volume-container-nautilus-1
      image: fedora:latest
      command: ["/bin/bash", "-c", "Sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/news
    - name: volume-container-nautilus-2
      image: fedora:latest
      command: ["/bin/bash", "-c", "Sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/apps
          
```
      
## Create pods using Kubectl   
        
```          
thor@jump_host ~$ kubectl apply -f /tmp/volume.yml
```
#### Output:
``` 
pod/volume-share-nautilus created
```
## Check Pods running successfully
```
thor@jump_host ~$ kubectl get pods
```
#### Output:
```
NAME                    READY   STATUS    RESTARTS   AGE
volume-share-nautilus   2/2     Running   0          1m7s
```
## You can validate by creating a text file under the mount path of the first container and checking if it's accessible from the second container
```
thor@jump_host ~$ kubectl exec -it volume-share-nautilus -c volume-container-nautilus-1 -- /bin/bash
```
```
[root@volume-share-nautilus /]# echo "Test for task validation" > /tmp/news/news.txt
```
```
[root@volume-share-nautilus /]# cat /tmp/news/news.txt 
```
#### Output:
```
Test for task validation
```

```
[root@volume-share-nautilus /]# exit
```

```
thor@jump_host ~$ kubectl exec -it volume-share-nautilus -c volume-container-nautilus-2 -- /bin/bash
```
```
[root@volume-share-nautilus /]# cat /tmp/apps/news.txt 
```
#### Output:
```
Test for task validation
```




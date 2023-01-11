
## To see if any deployments already exist, run the below command
```
thor@jump_host ~$ kubectl get deploy
```
#### Output:
```
No resources found in default namespace.
```
## Create a new deployment called httpd with the latest httpd image
```
thor@jump_host ~$ kubectl create deploy httpd --image httpd:latest
```
#### Output:
```
deployment.apps/httpd created
```
## List all deployments
```
thor@jump_host ~$ kubectl get deploy
```
#### Output:
```
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
httpd   1/1     1            1           15s
```
## List all pods
```
thor@jump_host ~$ kubectl get pods
```
#### Output:
```
NAME                    READY   STATUS    RESTARTS   AGE
httpd-84898796c-gbhrq   1/1     Running   0          21s
```
## Describe httpd pod
```
thor@jump_host ~$ kubectl describe pod httpd-84898796c-gbhrq
```
#### Output:

```
Name:         httpd-84898796c-gbhrq
Namespace:    default
Priority:     0
Node:         kodekloud-control-plane/172.17.0.2
Start Time:   Tue, 10 Jan 2023 21:15:04 +0000
Labels:       app=httpd
              pod-template-hash=84898796c
Annotations:  <none>
Status:       Running
IP:           10.244.0.5
IPs:
  IP:           10.244.0.5
Controlled By:  ReplicaSet/httpd-84898796c
Containers:
  httpd:
    Container ID:   containerd://c1fcfede5cfdcccf8ab4b8d3e02af7a9f6e46ac53cabef49be01289cb1c5b66a
    Image:          httpd:latest
    Image ID:       docker.io/library/httpd@sha256:f8c7bdfa89fb4448c95856c6145359f67dd447134018247609e7a23e5c5ec03a
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 10 Jan 2023 21:15:15 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-gpntb (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-gpntb:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-gpntb
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  56s   default-scheduler  Successfully assigned default/httpd-84898796c-gbhrq to kodekloud-control-plane
  Normal  Pulling    56s   kubelet            Pulling image "httpd:latest"
  Normal  Pulled     46s   kubelet            Successfully pulled image "httpd:latest" in 9.715544103s
  Normal  Created    46s   kubelet            Created container httpd
  Normal  Started    46s   kubelet            Started container httpd
```
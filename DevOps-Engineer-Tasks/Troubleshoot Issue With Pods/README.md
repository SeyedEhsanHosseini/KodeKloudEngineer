## List all pods  
```
thor@jump_host ~$ kubectl get pods
```

#### Output:
```
NAME        READY   STATUS             RESTARTS   AGE
webserver   1/2     ImagePullBackOff   0          36s
```
## Display the detailed state of a pod (in this task => webserver)
```
thor@jump_host ~$ kubectl describe pod webserver
```
#### Output:
```

Name:         webserver
Namespace:    default
Priority:     0
Node:         kodekloud-control-plane/172.17.0.2
Start Time:   Sat, 26 Nov 2022 22:52:13 +0000
Labels:       app=web-app
Annotations:  <none>
Status:       Pending
IP:           10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  nginx-container:
    Container ID:   
    Image:          nginx:latests
    Image ID:       
    Port:           <none>
    Host Port:      <none>
    State:          Waiting
      Reason:       ImagePullBackOff
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/log/nginx from shared-logs (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-r755h (ro)
  sidecar-container:
    Container ID:  containerd://408414b1cd41f73657e4903fca75ddbcd894c9854c11595d9f607543e00a52be
    Image:         ubuntu:latest
    Image ID:      docker.io/library/ubuntu@sha256:4b1d0c4a2d2aaf63b37111f34eb9fa89fa1bf53dd6e4ca954d47caebca4005c2
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done
    State:          Running
      Started:      Sat, 26 Nov 2022 22:52:34 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/log/nginx from shared-logs (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-r755h (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  shared-logs:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  <unset>
  default-token-r755h:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-r755h
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  73s                default-scheduler  Successfully assigned default/webserver to kodekloud-control-plane
  Normal   Pulling    59s                kubelet            Pulling image "ubuntu:latest"
  Normal   Started    52s                kubelet            Started container sidecar-container
  Normal   Pulled     52s                kubelet            Successfully pulled image "ubuntu:latest" in 6.673282161s
  Normal   Created    52s                kubelet            Created container sidecar-container
  Normal   BackOff    28s (x3 over 52s)  kubelet            Back-off pulling image "nginx:latests"
  Warning  Failed     28s (x3 over 52s)  kubelet            Error: ImagePullBackOff
  Normal   Pulling    13s (x3 over 59s)  kubelet            Pulling image "nginx:latests"
  Warning  Failed     13s (x3 over 59s)  kubelet            Failed to pull image "nginx:latests": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/library/nginx:latests": failed to resolve reference "docker.io/library/nginx:latests": docker.io/library/nginx:latests: not found
  Warning  Failed     13s (x3 over 59s)  kubelet            Error: ErrImagePull
```
## We can see from the error message that the image's tag is misspelled, and we should correct it.
```
thor@jump_host ~$ kubectl edit pod webserver
```

```
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"web-app"},"name":"webserver","namespace":"default"},"spec":{"containers":[{"image":"nginx:latests","name":"nginx-container","volumeMounts":[{"mountPath":"/var/log/nginx","name":"shared-logs"}]},{"command":["sh","-c","while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"],"image":"ubuntu:latest","name":"sidecar-container","volumeMounts":[{"mountPath":"/var/log/nginx","name":"shared-logs"}]}],"volumes":[{"emptyDir":{},"name":"shared-logs"}]}}
  creationTimestamp: "2022-11-26T22:52:12Z"
  labels:
    app: web-app
  name: webserver
  namespace: default
  resourceVersion: "7995"
  uid: 8a3dd841-5e0f-4ea3-98f5-ddd35cb4d204
spec:
  containers:
  - image: nginx:latests                            # nginx:latests => nginx:latest
    imagePullPolicy: IfNotPresent
    name: nginx-container
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/log/nginx
      name: shared-logs
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-r755h
      readOnly: true
  - command:
    - sh
    - -c
    - while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep
      30; done
    image: ubuntu:latest
    imagePullPolicy: Always
    name: sidecar-container
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/log/nginx
      name: shared-logs
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-r755h
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: kodekloud-control-plane
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - emptyDir: {}
    name: shared-logs
  - name: default-token-r755h
    secret:
      defaultMode: 420
      secretName: default-token-r755h
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2022-11-26T22:52:13Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2022-11-26T22:52:13Z"
    message: 'containers with unready status: [nginx-container]'
    reason: ContainersNotReady
    status: "False"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2022-11-26T22:52:13Z"
    message: 'containers with unready status: [nginx-container]'
    reason: ContainersNotReady
    status: "False"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2022-11-26T22:52:13Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - image: nginx:latests
    imageID: ""
    lastState: {}
    name: nginx-container
    ready: false
    restartCount: 0
    started: false
    state:
      waiting:
        message: 'rpc error: code = NotFound desc = failed to pull and unpack image
          "docker.io/library/nginx:latests": failed to resolve reference "docker.io/library/nginx:latests":
          docker.io/library/nginx:latests: not found'
        reason: ErrImagePull
  - containerID: containerd://408414b1cd41f73657e4903fca75ddbcd894c9854c11595d9f607543e00a52be
    image: docker.io/library/ubuntu:latest
    imageID: docker.io/library/ubuntu@sha256:4b1d0c4a2d2aaf63b37111f34eb9fa89fa1bf53dd6e4ca954d47caebca4005c2
    lastState: {}
    name: sidecar-container
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2022-11-26T22:52:34Z"
  hostIP: 172.17.0.2
  phase: Pending
  podIP: 10.244.0.5
  podIPs:
  - ip: 10.244.0.5
  qosClass: BestEffort
  startTime: "2022-11-26T22:52:13Z"
```   
## Save and Exit 

#### Output:
```
 pod/webserver edited
```

## To validate the task, list all pods again
```
thor@jump_host ~$ kubectl get pods
```

#### Output:
```
NAME        READY   STATUS    RESTARTS   AGE
webserver   2/2     Running   0          88s  
  
```






























  
  
  

 
 
 
  


                
    
        




































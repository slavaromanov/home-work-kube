Missing:
  - Разберитесь почему все pod в namespace kube-system
восстановились после удаления. Укажите причину в описании PR
Hint: core-dns и, например, kube-apiserver, имеют
различия в механизме запуска и восстанавливаются по
разным причинам


Для запуска minikube (kvm2) на ArchLinux потребовались:
  - libvirtd.service
  - dnsmasq.service
  - firewalld.service


```YAML
# kubectl get pod web -o yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"web"},"name":"web","namespace":"default"},"spec":{"containers":[{"image":"docker.io/monodry/file_serve:latest","name":"web"}]}}
  creationTimestamp: "2019-12-17T17:01:42Z"
  labels:
    app: web
  name: web
  namespace: default
  resourceVersion: "1103"
  selfLink: /api/v1/namespaces/default/pods/web
  uid: 6629cf1e-49be-4e53-b148-0bc571920a8d
spec:
  containers:
  - image: docker.io/monodry/file_serve:latest
    imagePullPolicy: Always
    name: web
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-qchpv
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: minikube
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
  - name: default-token-qchpv
    secret:
      defaultMode: 420
      secretName: default-token-qchpv
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2019-12-17T17:01:43Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2019-12-17T17:08:18Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2019-12-17T17:08:18Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2019-12-17T17:01:42Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: docker://e8531588f057b551167296403e676d789b048c5c0d131751fe1fd4dbba5247ad
    image: monodry/file_serve:latest
    imageID: docker-pullable://monodry/file_serve@sha256:5186d86b651c78379c59bce44101d8233cba14f45b7380ad500fd60d91d0147e
    lastState: {}
    name: web
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2019-12-17T17:08:17Z"
  hostIP: 192.168.39.169
  phase: Running
  podIP: 172.17.0.4
  podIPs:
  - ip: 172.17.0.4
  qosClass: BestEffort
  startTime: "2019-12-17T17:01:43Z"
```

```
# kubectl describe pod web
Name:         web
Namespace:    default
Priority:     0
Node:         minikube/192.168.39.169
Start Time:   Tue, 17 Dec 2019 20:01:43 +0300
Labels:       app=web
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"web"},"name":"web","namespace":"default"},"spec":{"container...
Status:       Running
IP:           172.17.0.4
IPs:
  IP:  172.17.0.4
Containers:
  web:
    Container ID:   docker://e8531588f057b551167296403e676d789b048c5c0d131751fe1fd4dbba5247ad
    Image:          docker.io/monodry/file_serve:latest
    Image ID:       docker-pullable://monodry/file_serve@sha256:5186d86b651c78379c59bce44101d8233cba14f45b7380ad500fd60d91d0147e
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 17 Dec 2019 20:08:17 +0300
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-qchpv (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-qchpv:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-qchpv
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  10m    default-scheduler  Successfully assigned default/web to minikube
  Normal  Pulling    10m    kubelet, minikube  Pulling image "docker.io/monodry/file_serve:latest"
  Normal  Pulled     3m35s  kubelet, minikube  Successfully pulled image "docker.io/monodry/file_serve:latest"
  Normal  Created    3m33s  kubelet, minikube  Created container web
  Normal  Started    3m31s  kubelet, minikube  Started container web
```

```bash
# собираем `Hipster Shop`
git clone git@github.com:GoogleCloudPlatform/microservices-demo.git && \
  cd microservices-demo/src/frontend && \
  docker build --network=host -t fronted . &&
  docker push monodry/frontend:v0.0.1
```



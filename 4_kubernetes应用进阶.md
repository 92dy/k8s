## Pod探针

### 存活性探测
-  `kubectl  explain pods.spec.containers.livenessProbe`
```
~]# kubectl  explain pods.spec.containers.livenessProbe.exec  |less
apiVersion: v1
kind: Pod
metadata:
  name: liveness-exec-pod
  namespace: default
spec:
  containers:
    - name: liveness-exec-container
      image: busybox:latest
      imagePullPolicy: IfNotPresent
      command: ["/bin/sh", "-c", "touch /tmp/healthy; sleep 30; rm -rf  /tmp/healthy; sleep 3600"]
      livenessProbe:
        exec:
          command: ["test", "-e",  " /tmp/healthy"]
        initialDelaySeconds: 1
        periodSeconds: 3
```
```
apiVersion: v1
kind: Pod
metadata:
  name: liveness-httpget-pod
  namespace: default
spec:
  containers:
    - name: liveness-httpget-container
      image: ikubernetes/myapp:v1
      imagePullPolicy: IfNotPresent
      ports:
      - name: http
        containerPort: 80
      livenessProbe:
        httpGet:
           port: http
           path: /index.html
        initialDelaySeconds: 1
        periodSeconds: 3
[root@k8s-master mainfests]# kubectl  exec -it  liveness-httpget-pod  -- /bin/sh    
/ # rm -rf /usr/share/nginx/html/index.html
```

### 就序性探测
- ``
```
[root@k8s-master mainfests]# cat readness-httpget.yaml
apiVersion: v1
kind: Pod
metadata:
  name: readness-httpget-pod
  namespace: default
spec:
  containers:
    - name: readness-httpget-container
      image: ikubernetes/myapp:v1
      imagePullPolicy: IfNotPresent
      ports:
      - name: http
        containerPort: 80
      readinessProbe:
        httpGet:
           port: http
           path: /index.html
        initialDelaySeconds: 1
        periodSeconds: 3
```

### 启动后钩子
- `kubectl  explain pods.spec.containers.lifecycle`
```
# poststartpod
[root@k8s-master mainfests]# cat poststart.yaml
apiVersion: v1
kind: Pod
metadata:
  name: poststart-pod
  namespace: default
spec:
  containers:
  - name: busybox-httpd
    image: busybox:latest
    imagePullPolicy: IfNotPresent
    lifecycle:
      postStart:
        exec:
          command: ["/bin/sh", "-c", "echo Home_Page >>  /tmp/index.html"] #后执行
   # command: ["/bin/httpd"]
   # args: ["-f", "-h /data/web/html"]
    command: ['/bin/sh', '-c', 'sleep 3600']  #先执行
```

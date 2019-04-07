## 获取pod的资源清单定义
```
~]# kubectl  get pod app-deployment-64f5947657-w9cdx -o yaml
```
- API群组及其版本`apiVersion`：` kubectl  api-versions`
- 资源类别`Kind`：`kubectl  explain pods.kind`
- 元数据`metadata`: `kubectl  explain pods.metadata`
  - name
  - namespace
  - label
  - annotations
- 期望状态`spec`：`kubectl  explain pods.spec`
- 当前状态`status`

```
mainfests]# cat pod-demo.yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-demo
  namespace: default
##lables: {app: myapp, tier: frontend}  
  labels:
    app: myapp
    tier: frontend
spec:
  containers:
  - name: app
    image: ikubernetes/myapp:v1
  - name: busybox
    image: busybox:latest
   #command: ["/bin/sh", "-c", "sleep 360000"]
    command:
    - "bin/sh"
    - "-c"
    - "sleep 3600000"
```

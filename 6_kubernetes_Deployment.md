## Deployment
上篇我们讲到ReplicaSet,这篇我们讲解Deployment，通过Deployment控制rc实现蓝绿发布或者金丝雀发布
- `kubectl  explain deploy`
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deploy
  namespace: default
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
      release: canary
  template:
    metadata:
      labels:
        app: myapp
        release: canary
    spec:
      containers:
      - name: myapp
        image: ikubernetes/myapp:v1
        ports:
        - name: http
          containerPort: 80
```
```
mainfests]# kubectl  patch deployment myapp-deploy -p '{"spec": {"replicas":5}}'  
mainfests]# kubectl  patch deployment myapp-deploy -p '{"spec": {"strategy": {"rollingUpdate":{"maxSurge":1, "MaxUnavalible":0}}}}'
mainfests]# kubectl  set image deployment myapp-deploy myapp=ikubernetes/myapp:v3 && kubectl  rollout pause deployment myapp-deploy
 ~]# kubectl  rollout status deployment myapp-deploy
mainfests]# kubectl  rollout resume deployment myapp-deploy
mainfests]# kubectl  rollout history deployment myapp-deploy
mainfests]# kubectl  rollout undo deployment myapp-deploy --to-revision=1
```

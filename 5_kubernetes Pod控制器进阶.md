## Pod 类型
### ReplicaSet-用与管控pod,其由Deployment来管控
- `~]# kubectl  explain rs`
```
mainfests]# cat rs-demo.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp
  namespace: default
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
      release: canary
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        release: canary
        environment: qa
    spec:
      containers:
      - name: myapp-contaainer
        image: ikubernetes/myapp:v1
        ports:
        - name: http
          containerPort: 80
```

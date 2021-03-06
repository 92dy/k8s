- `~]# kubectl  explain ds`
```
mainfests]# cat ds-demo.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
      role: logstorge
  template:
    metadata:
      labels:
        app: redis
        role: logstorge
    spec:
      containers:
      - name: redis
        image: redis:4.0-alpine
        ports:
        - name: redis
          containerPort: 6379
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: filebeat-daemonset
  namespace: default
spec:
  selector:
    matchLabels:
      app: filebeat
      release: stable
  template:
    metadata:
      labels:
        app: filebeat
        release: stable
    spec:
      containers:
      - name: filebeat
        image: ikubernetes/filebeat:5.6.5-alpine
        env:
        - name: REDIS_HOST
          value: redis.default.svc.cluster.local
        - name: REDIS_LOG_LEVEL
          value: info
```
```
 mainfests]# kubectl  expose deployment redis-deployment --port 6379
 mainfests]# kubectl  exec -it redis-deployment-59dff7fb84-jnk9q -- /bin/sh

```

- 依赖DNS：`kubectl  explain svc`
```
apiVersion: v1
kind: Service
metadata:
  name: redis
  namespace: default
spec:
  selector:
    app: redis
    role: logstorge
  clusterIP: 10.97.97.97
  type: ClusterIP
  ports:
  - port: 6379
    targetPort: 6379
```
SVC_NAME.NS_NAME.DOMAIN.LTD.
redis.default.svc.cluster.local.

```
apiVersion: v1
kind: Service
metadata:
  name: myapp-svc
  namespace: default
spec:
  selector:
    app: myapp
    release: canary
  clusterIP: 10.97.99.99
  sessionAffinity: ClientIP
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

```
apiVersion: v1
kind: Service
metadata:
  name: myapp-svc-headless
  namespace: default
spec:
  selector:
    app: myapp
    release: canary
  clusterIP: None
  ports:
  - port: 80
    targetPort: 80
```
dig -t A myapp-svc-headless.default.svc.cluster.local @10.96.0.10

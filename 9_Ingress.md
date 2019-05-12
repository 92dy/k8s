## 安装附件ingress
- `~]# kubectl  explain ingress`
- `~]# kubectl  explain ingress.spec`
- `~]# kubectl  explain ingress.spec.http`
- `https://github.com/kubernetes/ingress-nginx`

## 部署ingress contrallor
```
 ~]# wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml
```
```
~]# kubectl  apply -f mandatory.yaml
~]# kubectl  get pod -n ingress-nginx
```

## 部署后端
```
ingress]# cat deploy-demo.yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp
  namespace: default
spec:
  selector:
    app: myapp
    release: canary
  ports:
  - name: http
    targetPort: 80
    port: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deploy
  namespace: default
spec:
  replicas: 3
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
        image: ikubernetes/myapp:v2
        ports:
        - name: http
          containerPort: 80
```
## 添加服务接入集群外部流量
```
##部署服务监听节点端口
ingress]# wget  https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/provider/baremetal/service-nodeport.yaml
apiVersion: v1
kind: Service
metadata:
  name: ingress-nginx
  namespace: ingress-nginx
  labels:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
spec:
  type: NodePort
  ports:
    - name: http
      port: 80
      targetPort: 80
      protocol: TCP
      nodePort: 30080  #固定端口
    - name: https
      port: 443
      targetPort: 443
      protocol: TCP
      nodePort: 30443  #固定端口
  selector:
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
ingress]# kubectl  apply -f service-nodeport.yaml
service/ingress-nginx created
[root@k8s-master ingress]# kubectl  get svc -n ingress-nginx
---
```
## 添加ingress规则，代理至后端的pod(myapp)
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-myapp
  namespace: default
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
  - host: myapp.dengyou.com
    http:
      paths:
      - path:
        backend:
          serviceName: myapp
          servicePort: 80
ingress]# kubectl  apply -f ingress-myapp.yaml
ingress]# kubectl get ingress
NAME            HOSTS               ADDRESS   PORTS   AGE
ingress-myapp   myapp.dengyou.com             80      63s
```
```
ingress]# kubectl  get pod -n ingress-nginx
NAME                                        READY   STATUS    RESTARTS   AGE
nginx-ingress-controller-5694ccb578-wm8nn   1/1     Running   0          106m
ingress]# kubectl  exec -n ingress-nginx -it nginx-ingress-controller-5694ccb578-wm8nn -- /bin/sh
$
```

```

```




https://kubernetes.io/docs/concepts/services-networking/ingress/

https://github.com/kubernetes/ingress-nginx/tree/master/deploy

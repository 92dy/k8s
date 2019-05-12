Pod资源：
- `kubectl  explain pod.spec.containers`
- `kubectl  explain pod.spec.containers.ports`

- `https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/`
Image Entrypoint	Image Cmd	Container command	Container args	Command run
[/ep-1]	[foo bar]	<not set>	<not set>	[ep-1 foo bar]
[/ep-1]	[foo bar]	[/ep-2]	<not set>	[ep-2]
[/ep-1]	[foo bar]	<not set>	[zoo boo]	[ep-1 zoo boo]
[/ep-1]	[foo bar]	[/ep-2]	[zoo boo]	[ep-2 zoo boo]

```
spec:
  containers:
  - name: <string>
    image: <string>
    imagePullPolicy:  Always|Never|IfNotPresent

```

- label：
  - `kubectl  get pod --show-labels`
  - ` kubectl  label pods  pod-demo  release=canary`
  - `kubectl  label pods pod-demo release=stable --overwrite`

许多资源支持内嵌字段：
- matchLabels:直接给定键值
- matchExpressions：基于给定的标签选择器

## 节点对资源的倾向性-`nodeSelector`-`nodeName`
```
mainfests]# kubectl  get nodes --show-labels
mainfests]# kubectl  label nodes k8s-node1 distype=ssd
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
    ports:
    - name: http
      containerPort: 80
    - name: https
      containerPort: 443
  - name: busybox
    image: busybox:latest
    imagePullPolicy: IfNotPresent
   #command: ["/bin/sh", "-c", "sleep 360000"]
    command:
    - "bin/sh"
    - "-c"
    - "sleep 3600000"
  nodeSelector:
    disktype: ssd
```

## 资源注解-`annotations`
```
metadata:
  ...
  annotations:
    magedu.com/created-by: "cluster admin"
  ...
```

## Pod的生命周期

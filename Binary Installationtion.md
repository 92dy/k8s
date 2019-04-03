## 服务器拓扑图

| Hostname | ServiceIP | Services |
| --- | --- |  --- |
| k8s-master | 192.168.1.10 | master、node、etcd |
| k8s-node1 | 192.168.1.21 | node、etcd |
| k8s-node2 | 192.168.1.22 | node、etcd |

> **master** 组件：kube-apiserver、kube-controller-manager、kube-scheduler

> **Node** 组件：kubelet、kube-proxy、docker



cat > /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": ["https://o4uba187.mirror.aliyuncs.com"],
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF
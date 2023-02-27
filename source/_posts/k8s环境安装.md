---
title: k8s环境安装
date: 2023-02-27 15:26:19
tags:
- k8s
- k9s
- docker
---

## 服务器环境说明

> hostnamectl set-hostname k8s-master
10.211.55.7 k8s-master
10.211.55.10 k8s-node1
10.211.55.11 k8s-node2

```bash
yum install docker-ce-19.03.0 docker-ce-cli-19.03.0 containerd.io docker-compose-plugin

vim /etc/docker/daemon.json
{
  "registry-mirrors": ["https://dgv874d5.mirror.aliyuncs.com"],
  "exec-opts": ["native.cgroupdriver=systemd"]
}

systemctl daemon-reload && systemctl restart docker

tee ./images.sh <<-'EOF'
#!/bin/bash
images=(
kube-apiserver:v1.20.9
kube-proxy:v1.20.9
kube-controller-manager:v1.20.9
kube-scheduler:v1.20.9
coredns:1.7.0
etcd:3.4.13-0
pause:3.2
)
for imageName in ${images[@]} ; do
docker pull registry.cn-hangzhou.aliyuncs.com/lfy_k8s_images/$imageName
done
EOF

chmod +x ./images.sh && ./images.sh

journalctl -xfu kubelet

kubeadm init \
--apiserver-advertise-address=10.211.55.7 \
--image-repository registry.cn-hangzhou.aliyuncs.com/lfy_k8s_images \
--kubernetes-version v1.20.9 \
--service-cidr=10.96.0.0/16 \
--pod-network-cidr=192.168.0.0/16

mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf


curl https://projectcalico.docs.tigera.io/archive/v3.22/manifests/calico.yaml -O
kubectl apply -f calico.yaml

kubeadm join 10.211.55.7:6443 --token ll2p7o.f9d8gs0zb1cu6oqc --discovery-token-ca-cert-hash sha256:767f65134e2e3b3cdba21618bf63db25945136da41be5022db72a7901e460104


kubectl create deployment nginx --image=nginx --port=80 --replicas=2
kubectl expose deployment nginx --port=80
```
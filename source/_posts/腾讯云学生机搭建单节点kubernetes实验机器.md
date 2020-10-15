---
title: 腾讯云学生机搭建单节点kubernetes实验机器
date: 2020-10-15 13:46:11
tags:
- docker
- kubernetes
categories: docker
thumbnail: https://benz-blog.oss-cn-beijing.aliyuncs.com/kubernetes.png
---
# 环境
腾讯云学生机10元一月，CPU 1核，Memory 2G（实际上kubernetes master节点的最低配置需要双核2G内存）。预装的系统为CentOS7.6。
# 安装过程
## 环境准备
不知道为什么腾讯云默认的DNS无法访问git等网页，需要替换/etc/resolv.conf中的DNS最好替换为114(但是yum的时候因为源是腾讯的镜像所以会出现无法解析的问题到时候再换回默认的DNS)

同时还要关闭Swap(交换内存)
```bash
swapoff -a
```
## docker安装
```bash
yum update
yum install docker -y
systemctl start docker
systemctl enable docker
```
## k8s安装
### 配置阿里源
```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF
```
### 关闭selinux
```bash
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```
### 安装kubeadm, kubelet和kubectl
```bash
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable kubelet && systemctl start kubelet
```
### 在centos系统上设置iptables
```bash
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system
```
后面的执行过程中发现需要net.ipv4.ip_forward开启，修改/etc/sysctl.conf文件。
```bash
net.ipv4.ip_forward=1
```
保存后再sysctl --system执行一遍
### 创建虚拟网卡
由于vps没有公网ip，kubeadm部署k8s无法指定公网IP，当使用--apiserver-advertise-address=<public_ip>绑定公网ip时候会出现如下错误,原因是因为etcd服务无法绑定上指定的ip导致docker启动失败
```bash
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[kubelet-check] Initial timeout of 40s passed.
```
解决办法创建虚拟网卡
```bash 
cd /etc/sysconfig/network-scripts/
vi ifcfg-eth0:1
```
写入如下内容,xxx.xxx.xxx.xxx内容使用服务器对应的公网ip代替
```bash
DEVICE=eth0:1
BOOTPROTO=static
IPADDR=xxx.xxx.xxx.xxx
NETMASK=255.255.255.255
ONBOOT=yes
```
保存后退出
```bash
ifup eth0:1
```

### kubeadm init执行初始化
由于学生机只有一个cup所以必须要忽略掉cpu报错。--ignore-preflight-errors=NumCPU 。xxx.xxx.xxx.xxx使用你的公网ip来代替
```bash
kubeadm init --apiserver-advertise-address=xxx.xxx.xxx.xxx --ignore-preflight-errors=NumCPU --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers  #这句我没有实验过但是在文档中可以用--image-repository修改镜像地址。
```
或者使用参考[链接](https://yanyixing.github.io/2018/12/08/install-k8s/)拉去镜像并替换镜子的方法。然后使用如下命令
```bash
kubeadm init --apiserver-advertise-address=xxx.xxx.xxx.xxx --ignore-preflight-errors=NumCPU 
```
### 配置kubectl 中的config
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
### 安装Weave Net
```bash
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```
说是使用这条命令就能执行完成，但是我在第一次实验的时候失败了。所以第二次我先去下载了https://cloud.weave.works/k8s/net?k8s-version=(对应的版本)是个yaml文件，里面提到image先pull好然后在执行上述命令

### 允许Master Node 运行容器
```bash
kubectl taint nodes --all node-role.kubernetes.io/master-
```
### 查看
```bash
[kube@kube ~]$ kubectl get node
NAME   STATUS   ROLES    AGE   VERSION
kube   Ready    master   41m   v1.13.0
[kube@kube ~]$ kubectl get pods --all-namespaces
NAMESPACE     NAME                           READY   STATUS    RESTARTS   AGE
kube-system   coredns-86c58d9df4-89xn5       1/1     Running   0          41m
kube-system   coredns-86c58d9df4-mb96l       1/1     Running   0          41m
kube-system   etcd-kube                      1/1     Running   2          40m
kube-system   kube-apiserver-kube            1/1     Running   2          40m
kube-system   kube-controller-manager-kube   1/1     Running   1          40m
kube-system   kube-proxy-tgk25               1/1     Running   0          41m
kube-system   kube-scheduler-kube            1/1     Running   1          40m
kube-system   weave-net-csgmh                2/2     Running   0          27m

```
### 在本机上配置kubectl
如果本机之前没有连接过其他的kubernetes集群直接把.kube文件夹复制过去到Home目录下就可以。

如果之前有连接过需要把kube里的config文件结合进去，然后下面命令来切换你要管理的集群
```bash
 kubectl config use-context xxx
```
# 参考连接
- [通过kubeadm搭建单节点k8s环境](https://yanyixing.github.io/2018/12/08/install-k8s/)
- [centos系统 vps添加额外的ip](http://blog.vpsnm.com/624.html)
- [多K8s集群切换：Kubectl客户端配置全记录](https://my.oschina.net/u/3828502/blog/4253619)
- [kubeadm init 官方文档](https://kubernetes.io/zh/docs/reference/setup-tools/kubeadm/kubeadm-init/)
- [kubeadm reset](https://k8smeetup.github.io/docs/reference/setup-tools/kubeadm/kubeadm-reset/)
- [解决阿里云ECS下kubeadm部署k8s无法指定公网IP](https://my.oschina.net/u/4389078/blog/3233116)
- [centos网络重启](https://www.cnblogs.com/lexus/archive/2012/02/23/2364252.html)



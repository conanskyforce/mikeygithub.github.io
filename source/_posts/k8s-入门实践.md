---
title: k8s 入门实践
date: 2020-07-07 11:05:00
index_img: /resource/img/k8s.jpeg
category: DevOps
tags: Kubernates
---

# 环境准备

- 至少2台 2核4G 的服务器 Cent OS 7.6 / 7.7 / 7.8

### 服务器信息 CentOS Linux release 7.8.2003 (Core)

````
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3
Thread(s) per core:    2
Core(s) per socket:    2
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 85
Model name:            Intel(R) Xeon(R) Gold 6266C CPU @ 3.00GHz
Stepping:              7
CPU MHz:               3000.000
BogoMIPS:              6000.00
Hypervisor vendor:     KVM
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              30976K
NUMA node0 CPU(s):     0-3
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc eagerfpu pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single ssbd ibrs ibpb stibp ibrs_enhanced fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm mpx avx512f avx512dq rdseed adx smap clflushopt clwb avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1 arat avx512_vnni md_clear spec_ctrl intel_stibp flush_l1d arch_capabilities

````


K8s

- kubelet

Docker 镜像

- etcd
- kube-proxy
- kube-apiserver
- kube-controller-manager
- kube-scheduler

# 安装Kubernates

- 修改 hostname,不能使用localhost
````
如果您需要修改 hostname，可执行如下指令：
# 修改 hostname
hostnamectl set-hostname your-new-host-name
# 查看修改结果
hostnamectl status
# 设置 hostname 解析
echo "127.0.0.1   $(hostname)" >> /etc/hosts
````

- 检查网络

````
[root@server-781e6bf7-bce3-4d09-adc8-2a169fdb8719 ~]# ip route show
default via 192.168.0.1 dev eth0 proto dhcp metric 100 
169.254.169.254 via 192.168.0.254 dev eth0 proto dhcp metric 100 
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 
172.18.0.0/16 dev br-66ad3449f59f proto kernel scope link src 172.18.0.1 
172.19.0.0/16 dev br-3d8dbba954bf proto kernel scope link src 172.19.0.1 
192.168.0.0/24 dev eth0 proto kernel scope link src 192.168.0.39 metric 100 
[root@server-781e6bf7-bce3-4d09-adc8-2a169fdb8719 ~]# ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether fa:16:3e:c1:b5:a5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.39/24 brd 192.168.0.255 scope global noprefixroute dynamic eth0
       valid_lft 53761sec preferred_lft 53761sec
    inet6 fe80::f816:3eff:fec1:b5a5/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether 02:42:54:bb:c6:75 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:54ff:febb:c675/64 scope link 
       valid_lft forever preferred_lft forever
4: br-66ad3449f59f: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ee:d9:c2:68 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-66ad3449f59f
       valid_lft forever preferred_lft forever
    inet6 fe80::42:eeff:fed9:c268/64 scope link 
       valid_lft forever preferred_lft forever
5: br-3d8dbba954bf: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:6c:a5:67:cc brd ff:ff:ff:ff:ff:ff
    inet 172.19.0.1/16 brd 172.19.255.255 scope global br-3d8dbba954bf
       valid_lft forever preferred_lft forever
    inet6 fe80::42:6cff:fea5:67cc/64 scope link 
       valid_lft forever preferred_lft forever
9: veth7dba01e@if8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-66ad3449f59f state UP group default 
    link/ether aa:7c:a6:ae:85:2c brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::a87c:a6ff:feae:852c/64 scope link 
       valid_lft forever preferred_lft forever
15: veth9f52d5f@if14: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master br-3d8dbba954bf state UP group default 
    link/ether e2:bd:35:53:c6:0c brd ff:ff:ff:ff:ff:ff link-netnsid 1
    inet6 fe80::e0bd:35ff:fe53:c60c/64 scope link 
       valid_lft forever preferred_lft forever
````

- 安装 Docker

- 安装 nfs-utils

- 安装 kubectl / kubeadm / kubelet


````
# 在 master 节点和 worker 节点都要执行
# 最后一个参数 1.18.6 用于指定 kubenetes 版本，支持所有 1.18.x 版本的安装
# 腾讯云 docker hub 镜像
# export REGISTRY_MIRROR="https://mirror.ccs.tencentyun.com"
# DaoCloud 镜像
# export REGISTRY_MIRROR="http://f1361db2.m.daocloud.io"
# 华为云镜像
# export REGISTRY_MIRROR="https://05f073ad3c0010ea0f4bc00b7105ec20.mirror.swr.myhuaweicloud.com"
# 阿里云 docker hub 镜像
export REGISTRY_MIRROR=https://registry.cn-hangzhou.aliyuncs.com
curl -sSL https://kuboard.cn/install-script/v1.18.x/install_kubelet.sh | sh -s 1.18.6
````

- 初始化 master 节点


````
# 只在 master 节点执行
# 替换 x.x.x.x 为 master 节点实际 IP（请使用内网 IP）
# export 命令只在当前 shell 会话中有效，开启新的 shell 窗口后，如果要继续安装过程，请重新执行此处的 export 命令
export MASTER_IP=x.x.x.x
# 替换 apiserver.demo 为 您想要的 dnsName
export APISERVER_NAME=apiserver.demo
# Kubernetes 容器组所在的网段，该网段安装完成后，由 kubernetes 创建，事先并不存在于您的物理网络中
export POD_SUBNET=10.100.0.1/16
echo "${MASTER_IP}    ${APISERVER_NAME}" >> /etc/hosts
curl -sSL https://kuboard.cn/install-script/v1.18.x/init_master.sh | sh -s 1.18.6
````

- 检查 master 初始化结果

````
# 只在 master 节点执行
# 执行如下命令，等待 3-10 分钟，直到所有的容器组处于 Running 状态
watch kubectl get pod -n kube-system -o wide
# 查看 master 节点初始化结果
kubectl get nodes -o wide
````


成功

````
Every 2.0s: kubectl get pod -n kube-system -o wide                                                                                                                                                                                               Tue Jul 28 17:31:56 2020

NAME                                       READY   STATUS     RESTARTS   AGE    IP             NODE     NOMINATED NODE   READINESS GATES
calico-kube-controllers-5b8b769fcd-dw2fq   0/1     Pending    0          99s    <none>         <none>   <none>           <none>
calico-node-gcw6r                          0/1     Init:0/3   0          99s    192.168.0.39   mikey    <none>           <none>
coredns-66db54ff7f-2f9qn                   0/1     Pending    0          99s    <none>         <none>   <none>           <none>
coredns-66db54ff7f-csm5w                   0/1     Pending    0          99s    <none>         <none>   <none>           <none>
etcd-mikey                                 1/1     Running    0          108s   192.168.0.39   mikey    <none>           <none>
kube-apiserver-mikey                       1/1     Running    0          108s   192.168.0.39   mikey    <none>           <none>
kube-controller-manager-mikey              1/1     Running    0          108s   192.168.0.39   mikey    <none>           <none>
kube-proxy-xhmgq                           1/1     Running    0          99s    192.168.0.39   mikey    <none>           <none>
kube-scheduler-mikey                       1/1     Running    0          108s   192.168.0.39   mikey    <none>           <none>

````

````
[root@server-781e6bf7-bce3-4d09-adc8-2a169fdb8719 ~]# kubectl get nodes -o wide
NAME    STATUS     ROLES    AGE     VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION                CONTAINER-RUNTIME
mikey   NotReady   master   2m18s   v1.18.6   192.168.0.39   <none>        CentOS Linux 7 (Core)   3.10.0-1062.12.1.el7.x86_64   docker://19.3.8

````

- 初始化 worker节点
>获得 join命令参数在 master 节点上执行

```kubeadm token create --print-join-command```
    
可获取kubeadm join 命令及参数，如下所示

kubeadm token create 命令的输出

>kubeadm join apiserver.demo:6443 --token mpfjma.4vjjg8flqihor4vt     --discovery-token-ca-cert-hash sha256:6f7a8e40a810323672de5eee6f4d19aa2dbdb38411845a1bf5dd63485c43d303
 
`该 token 的有效时间为 2 个小时，2小时内，您可以使用此 token 初始化任意数量的 worker 节点`


- 初始化worker

针对所有的 worker 节点执行

````
# 只在 worker 节点执行
# 替换 x.x.x.x 为 master 节点的内网 IP
export MASTER_IP=x.x.x.x
# 替换 apiserver.demo 为初始化 master 节点时所使用的 APISERVER_NAME
export APISERVER_NAME=apiserver.demo
echo "${MASTER_IP}    ${APISERVER_NAME}" >> /etc/hosts
# 替换为 master 节点上 kubeadm token create 命令的输出
kubeadm join apiserver.demo:6443 --token mpfjma.4vjjg8flqihor4vt     --discovery-token-ca-cert-hash sha256:031a838dcdb8a66e0a5ddb826a2ada6065d3f68d5ddfa97226270a0e56861160
````

出现问题`kubeadm reset`进行重置














# 参考资料

[单节点安装](https://kuboard.cn/install/install-k8s.html#%E6%96%87%E6%A1%A3%E7%89%B9%E7%82%B9)

[Kubernates 教学](https://kuboard.cn/learning/k8s-bg/what-is-k8s.html#%E5%9B%9E%E9%A1%BE)
---
layout: default
title: Kubernetes 실습 환경
parent: Kubernetes
grand_parent: Cloud
permalink: /docs/cloud/k8s-install-lab/
nav_order: 10
---

# Kubernetes 실습 환경
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
Kubernetes 실습 환경을 위해 아래와 같이 구성한다.
-   master 에 kubeadm, kubelet and kubectl 패키지 설치
-   master 로부터 worker1, worker2 복제하기
-   master 에서 kubectl init 하여 조인구문 생성 및 worker1,2 로 복제
-   worker1,2 에서 조인구문 실행

## 도커 실습에 사용되었던 컨테이너, 이미지 모두 삭제하기

```bash
root@docker1:~# docker container rm -f $(docker container ps -aq)
root@docker1:~# docker image prune -a
```

## 쿠버네티스 클러스터를 구성하기전 요구사항 확인

### A compatible Linux host

The Kubernetes project provides generic instructions for Linux distributions based on Debian and Red Hat, and those distributions without a package manager.

### 2 GB or more of RAM per machine

Any less will leave little room for your apps.

```bash
root@docker1:~# free
              total        used        free      shared  buff/cache   available
Mem:        4039328     1140088     1636088       34088     1263152     2608324
Swap:       2017276           0     2017276
root@docker1:~#
```

### 2 CPUs or more

```bash
root@docker1:~# lscpu
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              1
On-line CPU(s) list: 0
Thread(s) per core:  1
Core(s) per socket:  1
Socket(s):           1
NUMA node(s):        1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               140
Model name:          11th Gen Intel(R) Core(TM) i7-1165G7 @ 2.80GHz
Stepping:            1
CPU MHz:             2803.200
BogoMIPS:            5606.40
Hypervisor vendor:   KVM
Virtualization type: full
L1d cache:           48K
L1i cache:           32K
L2 cache:            1280K
L3 cache:            12288K
NUMA node0 CPU(s):   0
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq monitor ssse3 cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single fsgsbase avx2 invpcid rdseed clflushopt md_clear flush_l1d arch_capabilities
root@docker1:~#
```

### Full network connectivity between all machines in the cluster

Public or private network is fine.

### Unique hostname, MAC address, and product\_uuid for every node

```bash
    - hostname : # hostname
        # hostnamectl set-hostname master
        # nano /etc/hosts 
        .... (맨 아래에 다음 내용 추가 )
        192.168.137.101         master
        192.168.137.102         worker1
        192.168.137.103         worker2

        ctrl+o (저장) --> enter --> ctrl+x (종료)

        # cat /etc/hosts  
        192.168.137.101 master
        192.168.137.102 worker1
        192.168.137.103 worker2

    - MAC address : # ip addr , # ifconfig
    - product_uuid : # cat /sys/class/dmi/id/product_uuid 
```

## kubelet 의 적절한 동작을 위해서 swap을 사용하지 않는다.

```bash
# swapon && cat /etc/fstab 
# swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab     (stream editor: 일괄 문서 편집기) 
```

### SELinux 와 방화벽을 해제한다.

```bash
# setenforce  0  // redhat 계열
# ufw disable  // ubuntu  계열
```

### br\_netfilter 모듈을 load하고 관련된 네트워크 파라미터를 설정

Linux 노드의 iptables가 bridged traffic을 정확하게 확인하고 제어 할 수 있도록한다.

```bash
# cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
--------------------------------------------------------
# cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
--------------------------------------------------------
# sysctl --system
```

## kubeadm, kubelet and kubectl 패키지 설치하기

### Update the apt package index and install packages needed to use the Kubernetes apt repository:

```bash
    # apt-get update
    # apt-get install -y apt-transport-https ca-certificates curl

```

### Download the Google Cloud public signing key:

```bash
root@docker1:~# curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

```

### Add the Kubernetes apt repository:

```bash
root@docker1:~# echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main
```

### Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:

```bash
# apt-get update
# apt-get install -y kubelet=1.21.0-00 kubeadm=1.21.0-00 kubectl=1.21.0-00
# apt-mark hold kubelet kubeadm kubectl
```

## Configuring the kubelet & cgroup

cgroupfs를 컨테이너 런타임과 kubelet 에 의해서 제어할 수 있도록 구성

```bash
root@docker1:~# cat <<EOF | sudo tee /etc/docker/daemon.json
> {
>   "exec-opts": ["native.cgroupdriver=systemd"],
>   "log-driver": "json-file",
>   "log-opts": {
>     "max-size": "100m"
>   },
>   "storage-driver": "overlay2"
> }
> EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
root@docker1:~# cat /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
```

## 시스템 구성

```bash
# systemctl enable docker              (부팅시 자동으로 docker 시작)
# systemctl daemon-reload
# systemctl restart docker

# docker info 
```

## worker1, worker2 가상 머신 추가

### 생성을 위해서 master 가상 머신 종료

`root@docker1:~# poweroff`

### 가상 머신 CPU 수 수정

```bash
Oracle VM VirtualBox  관리자 
master 가상 머신 선택 
    --> "시스템" 글자를 클릭  
    --> 마더보드/프로세서/가속 중 프로세서 선택 
    --> 프로세서(탭) --> 프로세서 개수 2개로 수정  
    --> 확인 
```

### worker1 가상 머신 복제

```bash
Oracle VM VirtualBox  관리자 
master 가상 머신 선택 
    --> 우클릭 
    --> 복제 

    이름 : worker1
    MAC 주소 정책 : 모든 네트워크 어댑터의 새 MAC  주소 생성 

    --> 다음 
    --> 복제 방식 --> 완전한 복제 --> 복제 


worker1 가상 머신 선택 
    --> 시작 
    --> 로그인 -->  worker1  / ubuntu 
    --> 바탕화면 --> 우클릭 --> Open Teminal 

    $ su - root              <-- 관리자 root 로 전환 
    Password : ubuntu 

    # hostnamectl set-hostname  worker1 
    # hostname
    worker1 

    # ip addr 
    # nano /etc/netplan/00-installer-config.yaml 
    addresses: [192.168.137.101/24]
            --> 192.168.137.102/24

    ctrl+o (저장)  --> enter --> ctrl+x (종료 )

    # netplan apply 
    # ip addr 
    enp0s3
        inet 192.168.137.102/24
```

### worker2 가상 머신 복제

```bash
Oracle VM VirtualBox  관리자 
master 가상 머신 선택 
    --> 우클릭 
    --> 복제 

    이름 : worker2
    MAC 주소 정책 : 모든 네트워크 어댑터의 새 MAC  주소 생성 

    --> 다음 
    --> 복제 방식 --> 완전한 복제 --> 복제 


worker2 가상 머신 선택 
    --> 시작 
    --> 로그인 -->  worker1  / ubuntu 
    --> 바탕화면 --> 우클릭 --> Open Teminal 

    $ su - root 
    Password : ubuntu 

    # hostnamectl set-hostname  worker2 
    # hostname
    worker2

    # ip addr 
    # nano /etc/netplan/00-installer-config.yaml 
    addresses: [192.168.137.101/24]
            --> 192.168.137.103/24

    ctrl+o (저장) ---> enter ---> ctrl+x (종료)

    # netplan apply 
    # ip addr 
    enp0s3
        inet 192.168.137.103/24
```

### master 가상 머신 구동

```
Oracle VM VirtualBox  관리자 
master 가상 머신 선택 
    --> 시작 
```

## NatNetwork port-forwarding 설정

```bash
Oracle VM VirtualBox  관리자
파일 
    --> 환경설정 
    --> 네트워크 --> NatNetwork --> 톱니바퀴 (설정) --> 포트포워딩
    --> [+]

    이름      프로토콜   호스트IP          호스트 포트      게스트IP            게스트포트
    worker1 TCP      192.168.56.1       102        192.168.137.102     22
    worker2 TCP      192.168.56.1       103        192.168.137.103     22
```

## putty 연결 설정

### worker1 putty 연결 설정

```bash
putty.exe 실행 
    --> master 선택 --> Load

    Host Name :  192.168.56.1   Port:102
    Window --> Colours --> Default Foreground(글자색) ,Default Background(배경색)
    Session --> Saved Sessions : worker1 --> Save

    worker1 연결 

    Accept

    login as : root  
    Password : [ubuntu] 
```

### worker2 putty 연결 설정

```bash
putty.exe 실행 
    --> master 선택 --> Load

    Host Name :  192.168.56.1   Port:103
    Window --> Colours --> Default Foreground(글자색) 
    Session --> Saved Sessions : worker2 --> Save

    worker2 연결 

    Accept

    login as : root  
    Password : [ubuntu] 
```

## (마스터 노드에서만 실행 )kubeadm init 명령을 통해서 클러스터를 생성한다.

### kubeadm init 명령어로 마스터 서버를 만든다.

```bash
root@master:~# kubeadm init
I0112 13:27:59.287535    4305 version.go:254] remote version is much newer: v1.2            6.0; falling back to: stable-1.21
[init] Using Kubernetes version: v1.21.14
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
... 중략
Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.137.101:6443 --token d77u1g.yknmv3qjjf6knxf1 \
        --discovery-token-ca-cert-hash sha256:c78641cda30821182794b94198cf0a17144cebbb4b1ae54daff71f9fd48f91d0
```

## 쿠버네티스 클러스터에 조인하기 위한 명령어 구문을 저장

```bash
root@master:~# cat > token.sh
kubeadm join 192.168.137.101:6443 --token d77u1g.yknmv3qjjf6knxf1 \
        --discovery-token-ca-cert-hash sha256:c78641cda30821182794b94198cf0a17144cebbb4b1ae54daff71f9fd48f91d0
root@master:~#
root@master:~#
root@master:~# cat token.sh
kubeadm join 192.168.137.101:6443 --token d77u1g.yknmv3qjjf6knxf1 \
        --discovery-token-ca-cert-hash sha256:c78641cda30821182794b94198cf0a17144cebbb4b1ae54daff71f9fd48f91d0
```

## kubeconfig 파일의 위치를 KUBECONFIG 쉘 변수에 설정

root 사용자가 쿠버네티스 클러스터의 API에 접근할 수 있도록 인증하기 위함이다.

```bash
# nano ~/.bashrc
export KUBECONFIG=/etc/kubernetes/admin.conf      <---- 파일 맨 아래 줄에 추가한다.

ctrl+o (저장) --> enter --> ctrl+x (종료)

# source ~/.bashrc 
# echo $KUBECONFIG 
/etc/kubernetes/admin.conf
```

### Pod 네트워크 추가 기능 구성

Pod가 서로 통신 할 수 있도록 CNI (Container Network Interface) 기반 Pod 네트워크 추가 기능 구성한다.  
네트워크가 설치되기 전에 클러스터 DNS (CoreDNS)가 시작되지 않는다.

```bash
 calico (https://docs.projectcalico.org/getting-started/kubernetes/self-managed-onprem/onpremises)
# wget https://docs.projectcalico.org/manifests/calico.yaml 
# kubectl apply -f calico.yaml
```

> 안될땐 아래  
> kubectl apply -f [https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml](https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml)

## \[마스터 노드에서만 실행\] 클러스터 구성 상태 확인

```
root@master:~# kubectl get nodes
NAME     STATUS   ROLES                  AGE   VERSION
master   Ready    control-plane,master   10m   v1.21.0
root@master:~# kubectl get pods --all-namespaces
NAMESPACE     NAME                                       READY   STATUS    RESTARTS   AGE
kube-system   calico-kube-controllers-846d7f49d8-hmc2h   1/1     Running   0          4m47s
kube-system   calico-node-2tvpz                          1/1     Running   0          4m47s
kube-system   coredns-558bd4d5db-cnnwp                   1/1     Running   0          9m52s
kube-system   coredns-558bd4d5db-skncw                   1/1     Running   0          9m52s
kube-system   etcd-master                                1/1     Running   0          10m
kube-system   kube-apiserver-master                      1/1     Running   0          10m
kube-system   kube-controller-manager-master             1/1     Running   0          10m
kube-system   kube-proxy-ln6sg                           1/1     Running   0          9m52s
kube-system   kube-scheduler-master                      1/1     Running   0          10m
root@master:~#
```

## \[마스터 노드에서만 실행\] 생성한 # kubeadm join 구문이 든 쉘 스크립트를 워커 노드로 복사

```bash
root@master:~# scp token.sh worker1:/root/token.sh
The authenticity of host 'worker1 (192.168.137.102)' can't be established.
ECDSA key fingerprint is SHA256:BYy8+Ddey03hnid+vAJg4KqojvmM1PMVQK0zZReh71s.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'worker1,192.168.137.102' (ECDSA) to the list of known hosts.
root@worker1's password:
Permission denied, please try again.
root@worker1's password:
token.sh                                                                                        100%  179   130.5KB/s   00:00

root@master:~# scp token.sh worker2:/root/token.sh
The authenticity of host 'worker2 (192.168.137.103)' can't be established.
ECDSA key fingerprint is SHA256:BYy8+Ddey03hnid+vAJg4KqojvmM1PMVQK0zZReh71s.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'worker2,192.168.137.103' (ECDSA) to the list of known hosts.
root@worker2's password:
token.sh                                                                                        100%  179   102.4KB/s   00:00
```

## \[워커 노드에서만 실행\] 복사된 스크립트를 실행하여 클러스터에 조인

`worker1, worker2 # chmod +x token.sh`  
`worker1, worker2 # ./token.sh`

```bash
root@worker1:~# chmod +x token.sh
root@worker1:~# ./token.sh
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```

## \[마스터 노드에서만 실행\] 클러스터 조인 상태 확인

```bash
root@master:~# kubectl cluster-info
Kubernetes control plane is running at https://192.168.137.101:6443
CoreDNS is running at https://192.168.137.101:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

```bash
root@master:~# kubectl get nodes
NAME      STATUS     ROLES                  AGE   VERSION
master    Ready      control-plane,master   13m   v1.21.0
worker1   NotReady   <none>                 62s   v1.21.0
worker2   NotReady   <none>                 59s   v1.21.0
```

```bash
root@master:~# kubectl get pods --all-namespaces
NAMESPACE     NAME                                       READY   STATUS     RESTARTS   AGE
kube-system   calico-kube-controllers-846d7f49d8-hmc2h   1/1     Running    0          8m7s
kube-system   calico-node-2tvpz                          1/1     Running    0          8m7s
kube-system   calico-node-ndnrd                          0/1     Init:0/3   0          78s
kube-system   calico-node-s99pf                          0/1     Init:1/3   0          75s
kube-system   coredns-558bd4d5db-cnnwp                   1/1     Running    0          13m
kube-system   coredns-558bd4d5db-skncw                   1/1     Running    0          13m
kube-system   etcd-master                                1/1     Running    0          13m
kube-system   kube-apiserver-master                      1/1     Running    0          13m
kube-system   kube-controller-manager-master             1/1     Running    0          13m
kube-system   kube-proxy-cbkjv                           1/1     Running    0          75s
kube-system   kube-proxy-ln6sg                           1/1     Running    0          13m
kube-system   kube-proxy-rvdnm                           1/1     Running    0          78s
kube-system   kube-scheduler-master                      1/1     Running    0          13m
```

아래 그림을 보며 이해해 보자.
 
  
![](/assets/images/k8sinstall1.png)

*[쿠버네티스 아키텍쳐 (https://kubernetes.io/docs/concepts/architecture/)]*
{: .text-center }
  
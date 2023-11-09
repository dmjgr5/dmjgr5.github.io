---
layout: default
title: Object & Controller
parent: Kubernetes
grand_parent: Cloud
permalink: /docs/cloud/k8s-object-controller/
nav_order: 12
---

# Object & Controller
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---



컨네이너는 크게 오브젝트(Object) 와 오브젝트를 관리하는 Controller 로 나뉜다.

## Object

관리 대상이다.

### Pod

Pod 란 콩깍지라는 어원으로 어플리케이션 기본 실행 단위(컨테이너) 를 배포할 수 있는 가장 작은 단위를 말한다. 즉, 하나의 컨테이너 또는 그를 보조하는 컨테이너까지 포함하여 이를 Pod 라고 일컫는다.

### Service

파드는 일회성으로 제거 후 다시 생성되는데 이때 노드가 바뀔 경우 IP 도 달라지게 된다. 서비스로 인해 노드 외부에서는 `고정 IP 주소` 로 파드에 접근할 수 있다.


![](/assets/images/k8sobjectcontroller1.png)


### Volume

컨테이너가 삭제되더라고 데이터는 유지되어야 하므로 `볼륩` 을 통해 데이터를 관리한다. 도커의 볼륨과 의미는 동일하다.

-   PV (PersistantVolume)
-   PVC (PersistantVolumeClame)

### NameSpace

동일 물리 클러스터에서 논리적으로 구분된 가상 클러스터를 일컫는다.  
여러 팀이나 많은 사용자가 있을 경우 클러스터 내에서 여러 어플리케이션을 실행할 경우 사용한다.  
`default` 가 기본값이다.

```bash
root@master:~# kubectl get namespace
NAME              STATUS   AGE
default           Active   41h  // 기본 네임스페이스
kube-node-lease   Active   41h //  스케일링 관련 네임스페이스
kube-public       Active   41h // 모든 사용자가 읽기 가능한 네임스페이스
kube-system       Active   41h // 쿠버네티스 시스템에서 생성한 네임스페이스
```

추가/삭제 테스트

```bash
root@master:~# kubectl get ns
NAME              STATUS   AGE
default           Active   41h
kube-node-lease   Active   41h
kube-public       Active   41h
kube-system       Active   41h
root@master:~#
root@master:~# kubectl create ns test-system
namespace/test-system created
root@master:~#
root@master:~# kubectl get ns
NAME              STATUS   AGE
default           Active   42h
kube-node-lease   Active   42h
kube-public       Active   42h
kube-system       Active   42h
test-system       Active   4s
root@master:~#
root@master:~# kubectl delete ns test-system
namespace "test-system" deleted
root@master:~# kubectl get ns
\NAME              STATUS   AGE
default           Active   42h
kube-node-lease   Active   42h
kube-public       Active   42h
kube-system       Active   42h
root@master:~# \
```

```bash
root@master:~# kubectl config current-context
kubernetes-admin@kubernetes
root@master:~#
root@master:~# kubectl config get-contexts kubernetes-admin@kubernetes
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin

// namespace 수정
root@master:~# kubectl config set-context kubernetes-admin@kubernetes --namespace=kube-system
Context "kubernetes-admin@kubernetes" modified.
root@master:~#
root@master:~# kubectl config get-contexts kubernetes-admin@kubernetes
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   kube-system
root@master:~# kubectl get pods
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-846d7f49d8-hmc2h   1/1     Running   1          41h
calico-node-2tvpz                          1/1     Running   2          41h
calico-node-ndnrd                          1/1     Running   2          41h
calico-node-s99pf                          1/1     Running   1          41h
coredns-558bd4d5db-cnnwp                   1/1     Running   1          42h
coredns-558bd4d5db-skncw                   1/1     Running   1          42h
etcd-master                                1/1     Running   2          42h
kube-apiserver-master                      1/1     Running   2          42h
kube-controller-manager-master             1/1     Running   2          42h
kube-proxy-cbkjv                           1/1     Running   2          41h
kube-proxy-ln6sg                           1/1     Running   2          42h
kube-proxy-rvdnm                           1/1     Running   2          41h
kube-scheduler-master                      1/1     Running   2          42h
root@master:~#
root@master:~# kubectl config set-context kubernetes-admin@kubernetes --namespace=default
Context "kubernetes-admin@kubernetes" modified.
root@master:~# kubectl get pods
No resources found in default namespace.
```

## Controller

Object 를 관리하는 얘들이다.  
파드가 원하는 수만큼 배포/유지되도록 한다.

### ReplicaSet

지정한 수만큼의 파드가 항상 실행되도록 관리한다.

### Deployment

일정수의 파드 수 유지, 롤아웃/롤백, 배포 중 일시정지 등의 기능이 있다. 기존에는 Replicaset 을 사용했지만 현재는 Deployment 를 가장 많이 사용한다.

### StatefulSet

### DaemonSet

### Job

### CronJob
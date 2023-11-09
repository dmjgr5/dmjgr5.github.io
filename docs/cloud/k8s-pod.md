---
layout: default
title: Pod
parent: Kubernetes
grand_parent: Cloud
permalink: /docs/cloud/k8s-pod/
nav_order: 13
---

# Pod
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---



Pod 에 관해 설명한다.

## Pod 생명 주기
 
![](/assets/images/k8spod1.png)

*[Pod 및 컨테이너 ]*
{: .text-center }



`Pod` 란 콩깍지라는 어원으로 어플리케이션 기본 실행 단위(컨테이너) 를 배포할 수 있는 가장 작은 단위를 말한다. 즉, 하나의 컨테이너 또는 그를 보조하는 컨테이너까지 포함하여 이를 Pod 라고 일컫는다.

-   Pending
-   Running
-   Succeeded
-   Failed
-   Unknown

## Pod 생성하기

### Pod 관련 조회

#### `kubectl get pods`

pod 조회한다.

#### `kubectl describe pod [app]`

상세 조회

#### `kubectl delete pod --all`

모든 파드를 삭제

### 명령으로 Pod 생성

#### `kubectl run app --image takytaky/app:v1`

#### `kubectl delete pod app`

```bash
root@master:~# kubectl delete pod app
^[[A^[[5~pod "app" deleted
root@master:~# kubectl run app --image takytaky/app:v1
pod/app created
root@master:~# kubectl describe pod app
```

### YAML 파일 사용하여 Pod 생성

yaml 파일 설정은 아래와 같은 때를 예로 한다.

```bash
root@master:~/labfile/pod# cat app.yaml
apiVersion: v1
kind: Pod
metadata:
  name: app // 파드의 이름(Namespace 에서 유일한 식별정보)
  labels:
    app: webapp // 오브젝트를 식별하는 라벨(유일 X)
spec:
  containers:
  - image: takytaky/app:v1 // 컨테이너에 사용할 이미지
    name: app-container
    ports:
    - containerPort: 80 // 컨테이너에 접속할 포트 번호
      protocol: TCP
root@master:~/labfile/pod#
```

#### `kubectl apply -f app.yaml`

Pod 생성 명령어를 입력한다.

```bash
root@master:~/labfile/pod# kubectl apply -f app.yaml
pod/app created
root@master:~/labfile/pod#

// 확인
root@master:~/labfile/pod# kubectl get pods
NAME   READY   STATUS    RESTARTS   AGE
app    1/1     Running   0          17s
root@master:~/labfile/pod#
```

#### `kubectl delete -f app.yaml`

root@master:~/labfile/pod# kubectl delete -f app.yaml  
pod "app" deleted

## Pod 리소스 할당

### CPU

```bash
root@master:~/labfile/pod/pod-resources#  cat cpu-request-limit.yaml
apiVersion: v1
kind: Pod
metadata:
  name: cpu-demo
  namespace: cpu-example
spec:
  containers:
  - name: cpu-demo-ctr
    image: vish/stress
    resources:
      limits:
        cpu: "1"
      requests:
        cpu: "0.5"
    args:
    - -cpus
    - "2"
root@master:~/labfile/pod/pod-resources#
root@master:~/labfile/pod/pod-resources# kubectl create namespace cpu-example
namespace/cpu-example created
root@master:~/labfile/pod/pod-resources# kubectl get ns
NAME              STATUS   AGE
cpu-example       Active   5s
default           Active   43h
kube-node-lease   Active   43h
kube-public       Active   43h
kube-system       Active   43h
root@master:~/labfile/pod/pod-resources# kubectl apply -f cpu-request-limit.yaml
pod/cpu-demo created
```

worker1 에 생성됨

```bash
Every 1.0s: kubectl get pods -o wide -n cpu-example                                              master: Sat Jan 14 08:33:37 2023

NAME       READY   STATUS    RESTARTS   AGE     IP               NODE      NOMINATED NODE   READINESS GATES
cpu-demo   1/1     Running   0          2m46s   172.16.235.131   worker1   <none>           <none>
```

worker1 에서 `docker stats` 실시간 자원 사용량 조회

```bash
CONTAINER ID   NAME                                                                                   CPU %     MEM USAGE / LIMIT     MEM %     NET I/O   BLOCK I/O        PIDS
f82cf3210728   k8s_cpu-demo-ctr_cpu-demo_cpu-example_01bcc06c-4fb9-4d41-9ec0-8ed06f9a8f45_0           97.67%    1.191MiB / 3.852GiB   0.03%     0B / 0B   0B / 0B          5
7c06002bc43e   k8s_POD_cpu-demo_cpu-example_01bcc06c-4fb9-4d41-9ec0-8ed06f9a8f45_0                    0.00%     564KiB / 3.852GiB     0.01%     0B / 0B   0B / 0B          1
b2938116ec2a   k8s_calico-node_calico-node-ndnrd_kube-system_8f163faa-d00f-4ed4-8a43-70239433078b_2   2.20%     93.87MiB / 3.852GiB   2.38%     0B / 0B   30.5MB / 233kB   64
db016fb7b959   k8s_kube-proxy_kube-proxy-rvdnm_kube-system_b0fd824c-953a-4014-8397-419a58d57d6c_2     0.00%     16.69MiB / 3.852GiB   0.42%     0B / 0B   44MB / 12.3kB    7
2d58d5594367   k8s_POD_calico-node-ndnrd_kube-system_8f163faa-d00f-4ed4-8a43-70239433078b_2           0.00%     1.312MiB / 3.852GiB   0.03%     0B / 0B   627kB / 0B       1
6b4b7d59384d   k8s_POD_kube-proxy-rvdnm_kube-system_b0fd824c-953a-4014-8397-419a58d57d6c_2            0.00%     740KiB / 3.852GiB     0.02%     0B / 0B   0B / 0B          1
```

### Memory

```bash
root@master:~/labfile/pod/pod-resources# cat memory-request-limit.yaml
apiVersion: v1
kind: Pod
metadata:
  name: memory-demo
  namespace: mem-example
spec:
  containers:
  - name: memory-demo-ctr
    image: polinux/stress
    resources:
      limits:
        memory: "200Mi"
      requests:
        memory: "100Mi"
    command: ["stress"]
    args: ["--vm", "1", "--vm-bytes", "150M", "--vm-hang", "1"]
root@master:~/labfile/pod/pod-resources#

// NS 생성
root@master:~/labfile/pod/pod-resources# kubectl create ns mem-example
namespace/mem-example created
root@master:~/labfile/pod/pod-resources# kubectl get ns
NAME              STATUS   AGE
default           Active   43h
kube-node-lease   Active   43h
kube-public       Active   43h
kube-system       Active   43h
mem-example       Active   4s

// 컨테이너 생성
root@master:~/labfile/pod/pod-resources# kubectl apply -f memory-request-limit.yaml

pod/memory-demo created
root@master:~/labfile/pod/pod-resources#
root@master:~/labfile/pod/pod-resources#
```

컨테이너 생성 확인한다.

```bash
Every 1.0s: kubectl get pods -o wide -n mem-example                                              master: Sat Jan 14 08:39:18 2023

NAME          READY   STATUS    RESTARTS   AGE     IP              NODE      NOMINATED NODE   READINESS GATES
memory-demo   1/1     Running   0          2m43s   172.16.189.69   worker2   <none>           <none>
```

worker2 에서 메모리 사용량 확인한다.

```bash
root@worker2:~# docker stats --no-stream
CONTAINER ID   NAME                                                                                   CPU %     MEM USAGE / LIMIT     MEM %     NET I/O   BLOCK I/O        PIDS
373dff622661   k8s_memory-demo-ctr_memory-demo_mem-example_4a24635a-760d-4e8b-93f1-db0d45f4f7a8_0     15.75%    151.1MiB / 200MiB     75.54%    0B / 0B   0B / 0B          2
38ce1f8aecee   k8s_POD_memory-demo_mem-example_4a24635a-760d-4e8b-93f1-db0d45f4f7a8_0                 0.00%     628KiB / 3.852GiB     0.02%     0B / 0B   0B / 0B          1
8479dcbb9470   k8s_calico-node_calico-node-s99pf_kube-system_640bad34-5dc5-4c64-91cb-5b21aeee4dce_1   2.07%     91.51MiB / 3.852GiB   2.32%     0B / 0B   29.2MB / 233kB   63
ee914529a024   k8s_kube-proxy_kube-proxy-cbkjv_kube-system_ad42f097-9ee0-4659-b46f-d01757439edd_2     0.00%     16.6MiB / 3.852GiB    0.42%     0B / 0B   44MB / 12.3kB    7
db632c6cc766   k8s_POD_calico-node-s99pf_kube-system_640bad34-5dc5-4c64-91cb-5b21aeee4dce_2           0.00%     760KiB / 3.852GiB     0.02%     0B / 0B   0B / 0B          1
47f87181b42e   k8s_POD_kube-proxy-cbkjv_kube-system_ad42f097-9ee0-4659-b46f-d01757439edd_2            0.00%     1.305MiB / 3.852GiB   0.03%     0B / 0B   627kB / 0B       1
root@worker2:~#
```

## initContainer (초기화 컨테이너)

컨테이너가 실행되기 전에 파드를 초기화 할 목적으로 사용된다.

![](/assets/images/k8spod2.jpeg)

*[초기화 컨테이너 예시]*
{: .text-center }




```bash
root@master:~/labfile/pod# cat init-containers.yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-demo
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - name: workdir
      mountPath: /usr/share/nginx/html
  # These containers are run during pod initialization
  initContainers:
  - name: install
    image: busybox
    command:
    - wget
    - "-O"
    - "/work-dir/index.html"
    - http://info.cern.ch
    volumeMounts:
    - name: workdir
      mountPath: "/work-dir"
  dnsPolicy: Default
  volumes:
  - name: workdir
    emptyDir: {}


root@master:~/labfile/pod# kubectl apply -f init-containers.yaml
pod/init-demo created
root@master:~/labfile/pod#
root@master:~/labfile/pod# curl -sf http://172.16.235.132
<html><head></head><body><header>
<title>http://info.cern.ch</title>
</header>

<h1>http://info.cern.ch - home of the first website</h1>
<p>From here you can:</p>
<ul>
<li><a href="http://info.cern.ch/hypertext/WWW/TheProject.html">Browse the first website</a></li>
<li><a href="http://line-mode.cern.ch/www/hypertext/WWW/TheProject.html">Browse the first website using the line-mode browser simulator</a></li>
<li><a href="http://home.web.cern.ch/topics/birth-web">Learn about the birth of the web</a></li>
<li><a href="http://home.web.cern.ch/about">Learn about CERN, the physics laboratory where the web was born</a></li>
</ul>
</body></html>
```

## Static Pod (스태틱 파드)

apiserver 를 통하지 않고, kubelet 이 직접 실행하는 파드이며, 기본 경로는 `/etc/kubernetes/manifests` 이다.

```bash
root@master:~/labfile/pod# cd /etc/kubernetes/manifests
root@master:/etc/kubernetes/manifests# ll
total 24
drwxr-xr-x 2 root root 4096 Jan 12 13:29 ./
drwxr-xr-x 4 root root 4096 Jan 14 07:35 ../
-rw------- 1 root root 2195 Jan 12 13:29 etcd.yaml
-rw------- 1 root root 3978 Jan 12 13:29 kube-apiserver.yaml
-rw------- 1 root root 3351 Jan 12 13:29 kube-controller-manager.yaml
-rw------- 1 root root 1385 Jan 12 13:29 kube-scheduler.yaml
```

> master 시작 시 kubelet 데몬 실행에서 위 static pod 가 실행된다.
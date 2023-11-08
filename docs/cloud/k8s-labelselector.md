---
layout: default
title: Label & Selector
parent: Kubernetes
grand_parent: Cloud
permalink: /docs/cloud/k8s-labelselector/
nav_order: 11
---

# Label & Selector
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

Label & Selector 에 관해 설명한다.

## Labels

클러스트 내 오브젝트 생성 시의 메타 데이터로 설정한다. 키-값으로 구성되어 있으며 컨트롤러들이 관리할 파드를 선택하고 구분하기 위해 사용된다.

라벨이 서로 다른 yaml 4개를 준비한다.

```bash
root@master:~/labfile/label# ls
annotation.yaml  label02.yaml  label04.yaml
label01.yaml     label03.yaml  nodeselector.yaml
root@master:~/labfile/label# cat label01.yaml
apiVersion: v1
kind: Pod
metadata:
  name: label-app01
  labels:
    app: nodejs
    environment: develop
    release: beta
spec:
  containers:
    - name: nodejs
      image: takytaky/app:v1
      ports:
      - containerPort: 80
root@master:~/labfile/label#
root@master:~/labfile/label# cat label02.yaml
apiVersion: v1
kind: Pod
metadata:
  name: label-app02
  labels:
    app: nodejs
    environment: production
    release: beta
spec:
  containers:
    - name: nodejs
      image: takytaky/app:v1
      ports:
      - containerPort: 80
root@master:~/labfile/label#
root@master:~/labfile/label# cat label03.yaml
apiVersion: v1
kind: Pod
metadata:
  name: label-app03
  labels:
    app: nodejs
    environment: develop
    release: stable
spec:
  containers:
    - name: nodejs
      image: takytaky/app:v1
      ports:
      - containerPort: 80
root@master:~/labfile/label#
root@master:~/labfile/label# cat label04.yaml
apiVersion: v1
kind: Pod
metadata:
  name: label-app04
  labels:
    app: nodejs
    environment: production
    release: stable
spec:
  containers:
    - name: nodejs
      image: takytaky/app:v1
      ports:
      - containerPort: 80
root@master:~/labfile/label#
```



- 4개의 컨테이너를 생성한다.

`kubectl apply -f label01.yaml -f label02.yaml -f label03.yaml -f label04.yaml`


- pod 조회

`kubectl get pods --show-labels`


```bash
root@master:~/labfile/label# kubectl get pods --show-labels
NAME        READY   STATUS    RESTARTS   AGE   LABELS
init-demo   1/1     Running   0          14h   <none>
root@master:~/labfile/label#
root@master:~/labfile/label# kubectl apply -f label01.yaml -f label02.yaml -f la                                                  bel03.yaml -f label04.yaml
pod/label-app01 created
pod/label-app02 created
pod/label-app03 created
pod/label-app04 created
root@master:~/labfile/label#

// labels 조회

root@master:~/labfile/label# kubectl get pods --show-labels
NAME          READY   STATUS    RESTARTS   AGE   LABELS
init-demo     1/1     Running   0          14h   <none>
label-app01   1/1     Running   0          22s   app=nodejs,environment=develop,release=beta
label-app02   1/1     Running   0          22s   app=nodejs,environment=production,release=beta
label-app03   1/1     Running   0          21s   app=nodejs,environment=develop,release=stable
label-app04   1/1     Running   0          21s   app=nodejs,environment=production,release=stable
root@master:~/labfile/label#
```

// -l 옵션을 사용한 조회

```bash
root@master:~/labfile/label# kubectl get pods -l  environment=production,release=stable --show-labels
NAME          READY   STATUS    RESTARTS   AGE     LABELS
label-app04   1/1     Running   0          2m48s   app=nodejs,environment=production,release=stable
root@master:~/labfile/label# kubectl get pods -l "app=nodejs,environment notin (develop)"
NAME          READY   STATUS    RESTARTS   AGE
label-app02   1/1     Running   0          3m
label-app04   1/1     Running   0          2m59s
root@master:~/labfile/label# kubectl get pods -l "release" --show-labels
NAME          READY   STATUS    RESTARTS   AGE     LABELS
label-app01   1/1     Running   0          3m13s   app=nodejs,environment=develop,release=beta
label-app02   1/1     Running   0          3m13s   app=nodejs,environment=production,release=beta
label-app03   1/1     Running   0          3m12s   app=nodejs,environment=develop,release=stable
label-app04   1/1     Running   0          3m12s   app=nodejs,environment=production,release=stable
root@master:~/labfile/label#
```

## NodeSelector

Label 을 통해서 노드를 선택할 수 있다.

- `spec > nodeSelector 정의` in YAML

```bash
apiVersion: v1
kind: Pod
metadata:
  name: nodeselector-pod
spec:
  nodeSelector:
    disk: ssd
```

- 라벨과 함께 노드 정보를 보여준다.

`kubectl get nodes --show-labels`



- worker1 노드에 disk=ssd 라벨를 추가한다.

`kubectl label nodes worker1 disk=ssd`



```bash

// worker1 에 disk=ssd 라벨 정의

root@master:~/labfile/label# kubectl label nodes worker1 disk=ssd
node/worker1 labeled

// 확인
root@master:~/labfile/label# kubectl get nodes --show-labels
NAME      STATUS   ROLES                  AGE     VERSION   LABELS
master    Ready    control-plane,master   2d10h   v1.21.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=master,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node-role.kubernetes.io/master=,node.kubernetes.io/exclude-from-external-load-balancers=
worker1   Ready    <none>                 2d10h   v1.21.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,disk=ssd,kubernetes.io/arch=amd64,kubernetes.io/hostname=worker1,kubernetes.io/os=linux
worker2   Ready    <none>                 2d10h   v1.21.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=worker2,kubernetes.io/os=linux
root@master:~/labfile/label#
root@master:~/labfile/label# cat nodeselector.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nodeselector-pod
spec:
  nodeSelector:
    disk: ssd
  containers:
  - name: nodeselector-pod
    image: takytaky/app:v1
    ports:
    - containerPort: 80
root@master:~/labfile/label#

// 생성
root@master:~/labfile/label# kubectl apply -f nodeselector.yaml
pod/nodeselector-pod created
root@master:~/labfile/label# kubectl get pods
NAME               READY   STATUS    RESTARTS   AGE
nodeselector-pod   1/1     Running   0          24s

 // disk=ssd 로 지정된 worker1 에서 생성됨 확인
root@master:~/labfile/label# kubectl get pods -o wide
NAME               READY   STATUS    RESTARTS   AGE   IP               NODE      NOMINATED NODE   READINESS GATES
nodeselector-pod   1/1     Running   0          44s   172.16.235.136   worker1   <none>           <none>
root@master:~/labfile/label#
```
 

- annotation : 사용자가 오브젝트를 구분하고 필터링 하기 위한 용도이다.
 

- 파드 상세 조회
`kubectl describe pod annotation-nodejs`


- \-o json 의미는 output 을 json 포맷으로 한다.

 `kubectl get pod annotation-nodejs -o json`





- metadata.annotations 부분만 보여줄것

`kubectl get pod annotation-nodejs -o jsonpath='{.metadata.annotations}'`


```bash

root@master:~/labfile/label# cat annotation.yaml
apiVersion: v1
kind: Pod
metadata:
  name: annotation-nodejs
  labels:
    app: nodejs
  annotations:
    manufacturer: "takytaky"
    e-mail: "takytaky@example.com"
    release-version: "v1"
spec:
  containers:
  - name: nodejs
    image: takytaky/app:v1
    ports:
    - containerPort: 80
root@master:~/labfile/label#
root@master:~/labfile/label# kubectl apply -f annotation.yaml
pod/annotation-nodejs created
root@master:~/labfile/label#

// 파드 상세 조회
root@master:~/labfile/label# kubectl describe pod annotation-nodejs
Name:         annotation-nodejs
Namespace:    default
Priority:     0
Node:         worker1/192.168.137.102
Start Time:   Sun, 15 Jan 2023 00:10:46 +0000
Labels:       app=nodejs
Annotations:  cni.projectcalico.org/containerID: 3ab7969b5b88a8fc45a563d107daeb401ab1b6ddc0618ab3e04d3036ea8f9e7e
              cni.projectcalico.org/podIP: 172.16.235.137/32
              cni.projectcalico.org/podIPs: 172.16.235.137/32
              e-mail: takytaky@example.com
              manufacturer: takytaky
              release-version: v1
Status:       Running
IP:           172.16.235.137.. 
.. 중략

Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  27s   default-scheduler  Successfully assigned default/annotation-nodejs to worker1
  Normal  Pulled     26s   kubelet            Container image "takytaky/app:v1" already present on machine
  Normal  Created    26s   kubelet            Created container nodejs
  Normal  Started    26s   kubelet            Started container nodejs
root@master:~/labfile/label#
root@master:~/labfile/label# ^C
root@master:~/labfile/label# kubectl get pod annotation-nodejs -o json
{
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
        "annotations": {
            "cni.projectcalico.org/containerID": "3ab7969b5b88a8fc45a563d107daeb401ab1b6ddc0618ab3e04d3036ea8f9e7e",
            "cni.projectcalico.org/podIP": "172.16.235.137/32",
            "cni.projectcalico.org/podIPs": "172.16.235.137/32",
            "e-mail": "takytaky@example.com",
            "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"kind\":\"Pod\",\"metadata\":{\"annotations\":{\"e-mail\":\"takytaky@example.com\",\"manufacturer\":\"takytaky\",\"release-version\":\"v1\"},\"labels\":{\"app\":\"nodejs\"},\"name\":\"annotation-nodejs\",\"namespace\":\"default\"},\"spec\":{\"containers\":[{\"image\":\"takytaky/app:v1\",\"name\":\"nodejs\",\"ports\":[{\"containerPort\":80}]}]}}\n",
            "manufacturer": "takytaky",
            "release-version": "v1"
        },
        "creationTimestamp": "2023-01-15T00:10:46Z",
        "labels": {
            "app": "nodejs"
        },
        "name": "annotation-nodejs",
        "namespace": "default",
        "resourceVersion": "24929",
        "uid": "9283cd0a-907c-4e57-bb06-c5f01934d36e"
    },
    "spec": {
        "containers": [
            {
                "image": "takytaky/app:v1",
                "imagePullPolicy": "IfNotPresent",
                "name": "nodejs",
                "ports": [
                    {
                        "containerPort": 80,
                        "protocol": "TCP"
                    }
                ],
                "resources": {},
                "terminationMessagePath": "/dev/termination-log",
                "terminationMessagePolicy": "File",
                "volumeMounts": [
                    {
                        "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount",
                        "name": "kube-api-access-n2bwn",
                        "readOnly": true
                    }
                ]
            }
        ],
        "dnsPolicy": "ClusterFirst",
        "enableServiceLinks": true,
        "nodeName": "worker1",
        "preemptionPolicy": "PreemptLowerPriority",
        "priority": 0,
        "restartPolicy": "Always",
        "schedulerName": "default-scheduler",
        "securityContext": {},
        "serviceAccount": "default",
        "serviceAccountName": "default",
        "terminationGracePeriodSeconds": 30,
        "tolerations": [
            {
                "effect": "NoExecute",
                "key": "node.kubernetes.io/not-ready",
                "operator": "Exists",
                "tolerationSeconds": 300
            },
            {
                "effect": "NoExecute",
                "key": "node.kubernetes.io/unreachable",
                "operator": "Exists",
                "tolerationSeconds": 300
            }
        ],
        "volumes": [
            {
                "name": "kube-api-access-n2bwn",
                "projected": {
                    "defaultMode": 420,
                    "sources": [
                        {
                            "serviceAccountToken": {
                                "expirationSeconds": 3607,
                                "path": "token"
                            }
                        },
                        {
                            "configMap": {
                                "items": [
                                    {
                                        "key": "ca.crt",
                                        "path": "ca.crt"
                                    }
                                ],
                                "name": "kube-root-ca.crt"
                            }
                        },
                        {
                            "downwardAPI": {
                                "items": [
                                    {
                                        "fieldRef": {
                                            "apiVersion": "v1",
                                            "fieldPath": "metadata.namespace"
                                        },
                                        "path": "namespace"
                                    }
                                ]
                            }
                        }
                    ]
                }
            }
        ]
    },
    "status": {
        "conditions": [
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2023-01-15T00:10:46Z",
                "status": "True",
                "type": "Initialized"
            },
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2023-01-15T00:10:48Z",
                "status": "True",
                "type": "Ready"
            },
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2023-01-15T00:10:48Z",
                "status": "True",
                "type": "ContainersReady"
            },
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2023-01-15T00:10:46Z",
                "status": "True",
                "type": "PodScheduled"
            }
        ],
        "containerStatuses": [
            {
                "containerID": "docker://ff1ca04c7800af5f0c600aeff6d6a8131761cf826ea9e5a1e82c673011c82372",
                "image": "takytaky/app:v1",
                "imageID": "docker-pullable://takytaky/app@sha256:a0893a44ecbc0c74e022175caa64748077ed8a6502c4e35516df2797fc530804",
                "lastState": {},
                "name": "nodejs",
                "ready": true,
                "restartCount": 0,
                "started": true,
                "state": {
                    "running": {
                        "startedAt": "2023-01-15T00:10:48Z"
                    }
                }
            }
        ],
        "hostIP": "192.168.137.102",
        "phase": "Running",
        "podIP": "172.16.235.137",
        "podIPs": [
            {
                "ip": "172.16.235.137"
            }
        ],
        "qosClass": "BestEffort",
        "startTime": "2023-01-15T00:10:46Z"
    }
}
root@master:~/labfile/label# ^C

// metadata.annotations 부분만 보여줄것 

root@master:~/labfile/label# kubectl get pod annotation-nodejs -o jsonpath='{.metadata.annotations}'
{"cni.projectcalico.org/containerID":"3ab7969b5b88a8fc45a563d107daeb401ab1b6ddc0618ab3e04d3036ea8f9e7e","cni.projectcalico.org/podIP":"172.16.235.137/32","cni.projectcalico.org/podIPs":"172.16.235.137/32","e-mail":"takytaky@example.com","kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"v1\",\"kind\":\"Pod\",\"metadata\":{\"annotations\":{\"e-mail\":\"takytaky@example.com\",\"manufacturer\":\"takytaky\",\"release-version\":\"v1\"},\"labels\":{\"app\":\"nodejs\"},\"name\":\"annotation-nodejs\",\"namespace\":\"default\"},\"spec\":{\"containers\":[{\"image\":\"takytaky/app:v1\",\"name\":\"nodejs\",\"ports\":[{\"containerPort\":80}]}]}}\n","manufacturer":"takytaky","release-version":"v1"}root@master:~/labfile/label#
root@master:~/labfile/label# ^C
root@master:~/labfile/label#
root@master:~/labfile/label# apt install -y jq
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libjq1 libonig4
The following NEW packages will be installed:
  jq libjq1 libonig4
0 upgraded, 3 newly installed, 0 to remove and 87 not upgraded.
Need to get 276 kB of archives.
After this operation, 930 kB of additional disk space will be used.
Get:1 http://kr.archive.ubuntu.com/ubuntu bionic/universe amd64 libonig4 amd64 6.7.0-1 [119 kB]
Get:2 http://kr.archive.ubuntu.com/ubuntu bionic/universe amd64 libjq1 amd64 1.5+dfsg-2 [111 kB]
Get:3 http://kr.archive.ubuntu.com/ubuntu bionic/universe amd64 jq amd64 1.5+dfsg-2 [45.6 kB]
Fetched 276 kB in 2s (139 kB/s)
Selecting previously unselected package libonig4:amd64.
(Reading database ... 173940 files and directories currently installed.)
Preparing to unpack .../libonig4_6.7.0-1_amd64.deb ...
Unpacking libonig4:amd64 (6.7.0-1) ...
Selecting previously unselected package libjq1:amd64.
Preparing to unpack .../libjq1_1.5+dfsg-2_amd64.deb ...
Unpacking libjq1:amd64 (1.5+dfsg-2) ...
Selecting previously unselected package jq.
Preparing to unpack .../jq_1.5+dfsg-2_amd64.deb ...
Unpacking jq (1.5+dfsg-2) ...
Setting up libonig4:amd64 (6.7.0-1) ...
Setting up libjq1:amd64 (1.5+dfsg-2) ...
Setting up jq (1.5+dfsg-2) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
Processing triggers for libc-bin (2.27-3ubuntu1.5) ...
root@master:~/labfile/label# kubectl get pod annotation-nodejs -o jsonpath='{.metadata.annotations}' | jq
{
  "cni.projectcalico.org/containerID": "3ab7969b5b88a8fc45a563d107daeb401ab1b6ddc0618ab3e04d3036ea8f9e7e",
  "cni.projectcalico.org/podIP": "172.16.235.137/32",
  "cni.projectcalico.org/podIPs": "172.16.235.137/32",
  "e-mail": "takytaky@example.com",
  "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"kind\":\"Pod\",\"metadata\":{\"annotations\":{\"e-mail\":\"takytaky@example.com\",\"manufacturer\":\"takytaky\",\"release-version\":\"v1\"},\"labels\":{\"app\":\"nodejs\"},\"name\":\"annotation-nodejs\",\"namespace\":\"default\"},\"spec\":{\"containers\":[{\"image\":\"takytaky/app:v1\",\"name\":\"nodejs\",\"ports\":[{\"containerPort\":80}]}]}}\n",
  "manufacturer": "takytaky",
  "release-version": "v1"
}
```

{: .note-title }
> jq 사용법
>
> json 포맷에서의 색깔을 구분하여 보여준다.
> `# apt install -y jq` 설치 후 아래와 같이 jq 옵션을 추가한다.
> `# kubectl get pod annotation-nodejs -o jsonpath='{.metadata.annotations}' | jq`

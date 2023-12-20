---
layout: default
title: Helm Chart Installation
parent: Helm Chart
grand_parent: Cloud
permalink: /docs/cloud/helmchart-install/
nav_order: 10
---

# Helm Chart Installation
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---



## `get_helm.sh` 파일 만들기 

`https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3` 링크로부터 sh 파일을 만든다.

```
root@master:~# curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 11345  100 11345    0     0  78241      0 --:--:-- --:--:-- --:--:-- 78241
root@master:~#
```

## sh 파일 실행위한 권한 변경하기
``` 
root@master:~# ll | grep helm
-rw-r--r--  1 root root  11345 Jan 16 13:53 get_helm.sh
root@master:~#
root@master:~# chmod 700 get_helm.sh
root@master:~#
root@master:~# ll | grep helm
-rwx------  1 root root  11345 Jan 16 13:53 get_helm.sh*
root@master:~#
```

## helm3 설치

```
root@master:~# ./get_helm.sh
Downloading https://get.helm.sh/helm-v3.10.3-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
```

## 버전 확인하기

```
root@master:~# helm version
version.BuildInfo{Version:"v3.10.3", GitCommit:"835b7334cfe2e5e27870ab3ed4135f136eecc704", GitTreeState:"clean", GoVersion:"go1.18.9"}
```

## Helm chart repository  추가
```
root@master:~# helm repo add stable https://charts.helm.sh/stable
"stable" has been added to your repositories
root@master:~#
```

## chart list 출력
```
root@master:~# helm search repo stable
NAME                                    CHART VERSION   APP VERSION             DESCRIPTION                            
stable/acs-engine-autoscaler            2.2.2           2.1.1                   DEPRECATED Scales worker nodes within agent pools
stable/aerospike                        0.3.5           v4.5.0.5                DEPRECATED A Helm chart for Aerospike in Kubern...
```

## chart update
```
root@master:~# helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
root@master:~#
```

## 설치된 helm 차트 삭제 
 
`helm list`

release를 삭제하는 방법은 helm delete(또는 uninstall)명령어를 사용하면 됩니다.

`helm delete <Release 이름>`



## Helm Install

먼저 kubernetes 가 설치되어 있어야 한다. K8s 와 helm 연동을 위해 아래 자료를 참조하자.

[https://computingforgeeks.com/install-and-use-helm-3-on-kubernetes-cluster/](https://computingforgeeks.com/install-and-use-helm-3-on-kubernetes-cluster/)
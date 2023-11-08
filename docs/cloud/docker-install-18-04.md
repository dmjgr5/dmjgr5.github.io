---
layout: default
title: Ubuntu 에 Docker 설치
parent: Docker
grand_parent: Cloud
permalink: /docs/cloud/docker-install-18-04/
nav_order: 10
---

# Ubuntu 18.04 Docker 설치
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---



## Docker 설치 방법

apt 패키지 관리자를 이용하여 먼저 패키지 목록 업데이트를 한다.

```bash
$ sudo apt update -y
```

도커CE는 몇가지 패키지에 의존하므로, 아래 4가지 패키지를 설치한다.

```bash
$ sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

도커 패키지 저장소를 apt에 등록한다.

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

도커CE를 설치한다

```bash
$ sudo apt install -y docker-ce
```

도커를 시작한다

```bash
$ sudo systemctl start docker
```

도커 상태 확인한다.

```bash
$ sudo systemctl status docker
```

도커 버전 확인한다.

```bash
$ docker version
Client: Docker Engine - Community
 Version:           24.0.2
 API version:       1.43
 Go version:        go1.20.4
 Git commit:        cb74dfc
 Built:             Thu May 25 21:52:13 2023
 OS/Arch:           linux/amd64
 Context:           default
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied

```

위와 같이 출력되면 도커가 정상적으로 출력된 것이다.


## docker.sock 권한 부여

Jenkins Script를 통해 docker image를 만들고 배포하려고 할 때, Jenkins Agent를 구동하는 계정이 root가 아닌 일반 사용자계정이면 아래의 오류가 발생한다.

```bash
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.38/build?buildargs=%7B%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&session=klv1s7gcvua6o8j7srtt572zk&target=&ulimits=null&version=1: dial unix /var/run/docker.sock: connect: permission denied
```

### 방법1. docker.sock 권한 변경

```bash
Chmod 666 /var/run/docker.sock
```
 

특이사항 : 권한을 변경한 경우, 일시적인 조치 입니다.
docker 서비스를 재시작하면 권한이 660으로 돌아옵니다.
해당 방법은 권장하지 않습니다.

 

### 방법2. Docker 그룹을 만들어서. 사용자 추가하기

```bash
$ Sudo groupadd docker

$ sudo usermod -aG docker $USER

$ newgrp
```
 

해당 사용자 계정이 docker group 권한이 부여되었음을 확인하는 방법

```bash
$ id
[testhost@testhost ~]$ id
uid=1001(testhost) gid=1002(testhost) groups=1002(testhost),1003(docker)
```

```bash
$sudo chown root:docker /var/run/docker.sock
$ systemctl restart docker
```


![](/assets/images/docker1.png)

 

만약 docker 재시작 이후에 권한설정이 돌아온다면?

파일생성 : /etc/docker/daemon.json

 
```bash
{
"group" : "docker"
}
```
 

위 작업이 완료된 이후에 systemctl reboot 실행이 필요하다.

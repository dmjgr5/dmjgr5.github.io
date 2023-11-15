---
layout: default
title: Dockerfile
parent: Docker
grand_parent: Cloud
permalink: /docs/cloud/docker-dockerfile/
nav_order: 26
---

# Dockerfile
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
`Dockerfile`은 DockerImage를 생성하기 위한 스크립트(설정파일)이다. 여러가지 명령어를 토대로 `Dockerfile`을 작성한 후 빌드하면 Docker는 `Dockerfile` 에 나열된 명령문을 차례대로 수행하며 `DockerImage`를 생성해준다. `Dockerfile` 을 읽을 줄 안다는 것은 해당 이미지가 어떻게 구성되어 있는지 알 수 있다는 의미이다

## Dockerfile 주요 개념과 생성

  
![](/assets/images/dockerdockerfile1.png)

*[Dockerfile 개념(https://www.researchgate.net/figure/Example-of-a-fictitious-Dockerfile-illustrating-common-instructions-and-quality-issues_fig1_345667168)]*
{: .text-center }



### Dockerfile 을 통한 build 방법

`docker build` 명령어를 사용한다.

```bash
root@master:~/labfile/dockerfile_dir# cat Dockerfile
FROM centos
```

```bash
// FROM 센토스를 베이스 이미지로 지정한다.
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile
FROM centos
root@docker1:~/labfile/dockerfile_dir#

// docker build 실행
root@docker1:~/labfile/dockerfile_dir# docker build -t testimage -f Dockerfile .
Sending build context to Docker daemon  176.1kB
Step 1/1 : FROM centos
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
 ---> 5d0da3dc9764
Successfully built 5d0da3dc9764
Successfully tagged testimage:latest
root@docker1:~/labfile/dockerfile_dir#

// testimage 이미지 생성 확인
root@docker1:~/labfile/dockerfile_dir# docker image ls
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
testuser/web-nginx    1.0       6f10cf278ad2   24 hours ago    174MB
testuser/box          1.0       66ba00ad3de8   6 days ago      4.87MB
busybox             latest    66ba00ad3de8   6 days ago      4.87MB
testuser/web-server   1.0       1403e55ab369   2 weeks ago     142MB
nginx               latest    1403e55ab369   2 weeks ago     142MB
ubuntu              latest    6b7dfa7e8fdb   4 weeks ago     77.8MB
centos              latest    5d0da3dc9764   16 months ago   231MB
testimage           latest    5d0da3dc9764   16 months ago   231MB
root@docker1:~/labfile/dockerfile_dir#
```

> `-t` (tag) : 이미지 이름  
> `-f` (file) : Dockerfile 이름  
> `.` : Dockerfile 위치하는 디렉토리

### Dockerfile 과 Image layer

Dockerfile 의 명령별로 이미지 레이어가 생성된다  
\*\* 예) 베이스 이미지는 `ubuntu`, 그 위에 `nginx` 레이어로 두번째 이미지, 그 위에 `index.html` 이 올라가 세번째 이미지가 생성된다. \*\*

```bash
root@master:~/labfile/dockerfile_dir# cat Dockerfile.1
FROM ubuntu
RUN  apt-get update && apt-get install -y -q nginx && rm -rf /var/lib/apt/lists/*
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]

root@master:~/labfile/dockerfile_dir#
```

```bash
// index.html 파일 생성
root@docker1:~/labfile/dockerfile_dir# echo Hello World > index.html

// Dockerfile.1 소스 확인
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.1
FROM ubuntu
RUN  apt-get update && apt-get install -y -q nginx
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]
root@docker1:~/labfile/dockerfile_dir#

// docker build
root@docker1:~/labfile/dockerfile_dir# docker build -t web-nginx:v1 -f Dockerfile.1 .
Sending build context to Docker daemon  176.1kB
Step 1/4 : FROM ubuntu
 ---> 6b7dfa7e8fdb
Step 2/4 : RUN  apt-get update && apt-get install -y -q nginx
 ---> Running in 7e6fe0ef8c1b
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [786 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [114 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [99.8 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy/restricted amd64 Packages [164 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]
Get:8 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]
... 중략
Setting up nginx (1.18.0-6ubuntu14.3) ...
Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
Removing intermediate container 7e6fe0ef8c1b
 ---> 1edf77c6398f
Step 3/4 : COPY index.html /var/www/html/
 ---> eeb5a81c7c8c
Step 4/4 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in 3dda10f8fb0a
Removing intermediate container 3dda10f8fb0a
 ---> 63123ab8639e
Successfully built 63123ab8639e
Successfully tagged web-nginx:v1

// 이미지 생성 확인 (web-nginx:v1)
root@docker1:~/labfile/dockerfile_dir# docker image ls
REPOSITORY          TAG       IMAGE ID       CREATED              SIZE
web-nginx           v1        63123ab8639e   About a minute ago   174MB
testuser/web-nginx    1.0       6f10cf278ad2   24 hours ago         174MB
testuser/box          1.0       66ba00ad3de8   6 days ago           4.87MB
busybox             latest    66ba00ad3de8   6 days ago           4.87MB
nginx               latest    1403e55ab369   2 weeks ago          142MB
testuser/web-server   1.0       1403e55ab369   2 weeks ago          142MB
ubuntu              latest    6b7dfa7e8fdb   4 weeks ago          77.8MB
centos              latest    5d0da3dc9764   16 months ago        231MB
testimage           latest    5d0da3dc9764   16 months ago        231MB
```

> `FROM` : 기본 이미지  
> `RUN` : 내부에서 수행할 작업  
> `COPY` : 호스트에서 파일  
> `CMD` : 1번 프로세스에 대한 command 의 약자

### 이미지 생성 시 용량 관련 고려사항

용량이 작은 베이스 이미지 선택하거나 불필요한 파일들 삭제한다.

-   alpine , ###-slim, busybox
-   `rm -rf /var/lib/apt/lists/*`

```bash
root@docker1:~/labfile/dockerfile_dir# nano Dockerfile.1
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.1
FROM ubuntu
RUN  apt-get update && apt-get install -y -q nginx && rm -rf /var/lib/apt/lists/*
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]

root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# docker build -t web-nginx:v2 -f Dockerfile.1 .
Sending build context to Docker daemon  176.1kB
Step 1/4 : FROM ubuntu
 ---> 6b7dfa7e8fdb
Step 2/4 : RUN  apt-get update && apt-get install -y -q nginx && rm -rf /var/lib/apt/lists/*
 ---> Running in 72c6c189dc81
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
... 중략
Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
Removing intermediate container 72c6c189dc81
 ---> 24be474303f8
Step 3/4 : COPY index.html /var/www/html/
 ---> 9622834c67c5
Step 4/4 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in c75a2a490216
Removing intermediate container c75a2a490216
 ---> 5e2b37603055
Successfully built 5e2b37603055
Successfully tagged web-nginx:v2

root@docker1:~/labfile/dockerfile_dir# docker image ls
REPOSITORY          TAG       IMAGE ID       CREATED          SIZE
web-nginx           v2        5e2b37603055   18 seconds ago   133MB
web-nginx           v1        63123ab8639e   15 minutes ago   174MB
```

> /var/lib/apt/lists/\*\` 를 삭제하고 진행했을 때 v1 보다 사이즈가 작음을 확인할 수 있다.

## Dockerfile 주요 키워드

### RUN

빌드 과정에서 어플리케이션/미들웨어 설치, 환경 설정을 위한 명령들을 정의한다. (계정생성, 그룹생성 등..)

#### shell 형식 기술

자주 사용하는 방식으로 쉘 프롬프트에 명령을 기술하는 방식으로 다음과 같으며, 베이스 이미지 위에서 `/bin/sh -c` 실행과 동일하다.  
`RUN apt-get update && apt-get install -y -q nginx`

#### exec 형식 기술

쉘을 사용하지 않고 직접 실행하고 `$HOME` 같은 환경변수 사용가능하다. `JSON` 형식을 사용한다.  
`RUN ["/bin/bash", -C, "apt-get install -y nginx"]`

```bash
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.2
FROM ubuntu
RUN echo "Shell: Home Dir -->  $HOME " <-- shell 형식
RUN ["echo", "Exec: Home Dir --> $HOME" ] <-- exec 형식
RUN ["/bin/bash" , "-c", "echo Exec: Home Dir : $HOME" ] <-- exec 형식

root@docker1:~/labfile/dockerfile_dir# docker build -t echo00 -f Dockerfile.2 .
Sending build context to Docker daemon  176.1kB
Step 1/4 : FROM ubuntu
 ---> 6b7dfa7e8fdb
Step 2/4 : RUN echo "Shell: Home Dir -->  $HOME "
 ---> Running in 89fcdd57095c

 // shell 형식
Shell: Home Dir -->  /root
Removing intermediate container 89fcdd57095c
 ---> 959d899d68ac
Step 3/4 : RUN ["echo", "Exec: Home Dir --> $HOME" ]
 ---> Running in 9e24500dcd29
 // exec 형식
Exec: Home Dir --> $HOME
Removing intermediate container 9e24500dcd29
 ---> 543426d48390
Step 4/4 : RUN ["/bin/bash" , "-c", "echo Exec: Home Dir : $HOME" ]
 ---> Running in 29afed36471a
 // exec 형식
Exec: Home Dir : /root
Removing intermediate container 29afed36471a
 ---> 803e2dcf0ab0
Successfully built 803e2dcf0ab0
Successfully tagged echo00:latest
root@docker1:~/labfile/dockerfile_dir#

// 이미지 생성 확인
root@docker1:~/labfile/dockerfile_dir# docker image ls
REPOSITORY          TAG       IMAGE ID       CREATED          SIZE
echo00              latest    803e2dcf0ab0   5 minutes ago    77.8MB
```

### CMD

컨테이너 내부에서 수행될 작업이나 명령 즉, 컨테이너가 시작될 때 실행되는 프로그램을 정의한다. `Dockerfile` 하나에 하나의 `CMD` 만 기술해야 한다.

```bash
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.1
FROM ubuntu
RUN  apt-get update && apt-get install -y -q nginx
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]
```

> RUN 과 혼동스럽긴 한데 아래 예제로 체크해 보자.  
> Nginx 설치 시는 `RUN`, Nginx 데몬 실행 시는 `CMD`

### ENTRYPOINT

컨테이너가 시작될 때 실행할 작업이다. `CMD` 와 같이 사용되면 `CMD` 는 `ENTRYPOINT` 의 인수 형태로 먼저 해석된다.

```bash
// ENTRYPOINT 와 CMD 같이 정의됨.
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.3
FROM centos
ENTRYPOINT ["/bin/ping", "localhost"]
CMD ["-c", "3"]

root@docker1:~/labfile/dockerfile_dir#

// ping00 이미지 생성
root@docker1:~/labfile/dockerfile_dir# docker build -t ping00 -f Dockerfile.3 .
Sending build context to Docker daemon  176.1kB
Step 1/3 : FROM centos
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
 ---> 5d0da3dc9764
Step 2/3 : ENTRYPOINT ["/bin/ping", "localhost"]
 ---> Running in 50ce39c4451e
Removing intermediate container 50ce39c4451e
 ---> 5075db00e5d5
Step 3/3 : CMD ["-c", "3"]
 ---> Running in 8357b97afb7d
Removing intermediate container 8357b97afb7d
 ---> 02259b638b70
Successfully built 02259b638b70
Successfully tagged ping00:latest
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
ping00       latest    02259b638b70   2 minutes ago   231MB
centos       latest    5d0da3dc9764   16 months ago   231MB
root@docker1:~/labfile/dockerfile_dir#

// CMD ["-c", "3"] 에 의해 3번 PING 실행
root@docker1:~/labfile/dockerfile_dir# docker run -it ping00
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.023 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.036 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.040 ms

--- localhost ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2040ms
rtt min/avg/max/mdev = 0.023/0.033/0.040/0.007 ms
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#

// 인자 넣어주면 CMD 무시됨.
root@docker1:~/labfile/dockerfile_dir# docker run -it ping00 -c 5
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.050 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.025 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.048 ms
64 bytes from localhost (127.0.0.1): icmp_seq=4 ttl=64 time=0.079 ms
64 bytes from localhost (127.0.0.1): icmp_seq=5 ttl=64 time=0.031 ms

--- localhost ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4097ms
rtt min/avg/max/mdev = 0.025/0.046/0.079/0.020 ms
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#

// 원본 이미지 centos 는 아래와 같다.

root@docker1:~/labfile/dockerfile_dir# docker image inspect centos
[
    {
        "Id": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6", 
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/bin/bash\"]"
            ],

            "Entrypoint": null,

// ping00 은 아래와 같다.
 root@docker1:~/labfile/dockerfile_dir# docker image inspect ping00
[
    {
        "Id": "sha256:02259b638b7070d3cb7a082fc6274e991fe5ab8ad371c03409f77b0c149c5ec7",

            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"-c\" \"3\"]"
            ],

            "Entrypoint": [
                "/bin/ping",
                "localhost"
            ],
```

### ENV

Dockerfile 안에서 컨테이너 환경 변수를 설정한다. 예로서 웹에서 DB정보 연결할 때 사용한다. 자주 사용되는 키워드이다.

### LABEL

이미지에 대한 부가적인 설명을 제공한다.

```bash
// 도커파일 정의
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.4
FROM centos

ENV myName “Yu Mi”
ENV myOrder Pizza Pasta Salad
ENV myNumber 1004

LABEL maintainer "Yu Mi <yumi@example.com>"
LABEL title "TEST Image"
LABEL version 1.0
LABEL description "This image is test image ^__^"

// 이미지 빌드
root@docker1:~/labfile/dockerfile_dir# docker build -t param00 -f Dockerfile.4 .
Sending build context to Docker daemon  176.1kB
Step 1/8 : FROM centos
latest: Pulling from library/centos
.. 중략
Successfully built e7bf5be9766f
Successfully tagged param00:latest
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# docker image inspect param00
[
    {
        "Id": "sha256:e7bf5be9766f77b2bf1d9536aead63c6bd528ad9466bbcb257625c641b93ab51",
       ... 중략
        "ContainerConfig": {

            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "myName=“Yu Mi”",
                "myOrder=Pizza Pasta Salad",
                "myNumber=1004"
            ],
      ... 중략
            "Labels": {
                "description": "This image is test image ^__^",
                "maintainer": "Yu Mi <yumi@example.com>",
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS",
                "title": "TEST Image",
                "version": "1.0"
            }
        },

```

컨테이너 안에서 환경변수를 확인해 보자.

```bash
// param00 컨테이너 실행
root@docker1:~/labfile/dockerfile_dir# docker run -it --rm param00
[root@4a5fa49b48bc /]# echo $myName : $myOrder : $myNumber
“Yu Mi” : Pizza Pasta Salad : 1004
[root@4a5fa49b48bc /]#
[root@4a5fa49b48bc /]# exit
exit

// --env 로 컨테이너 생성 시 정의
root@docker1:~/labfile/dockerfile_dir# docker run -it --env myAge=20 --env myNumber=1005 param00
[root@1b02c26cfcd8 /]#  echo $myAge
20
[root@1b02c26cfcd8 /]# echo $myNumber
1005
[root@1b02c26cfcd8 /]# env
myName=“Yu Mi”
LANG=en_US.UTF-8
HOSTNAME=1b02c26cfcd8
PWD=/
HOME=/root
myOrder=Pizza Pasta Salad
myAge=20
TERM=xterm
SHLVL=1
myNumber=1005
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LESSOPEN=||/usr/bin/lesspipe.sh %s
_=/usr/bin/env
[root@1b02c26cfcd8 /]#
[root@1b02c26cfcd8 /]# exit
exit
root@docker1:~/labfile/dockerfile_dir#
```

### ADD , COPY

`ADD`와 `COPY`는 특정 위치에서 Docker 이미지로 파일을 복사하는 기능을 수행하는 Dockerfile 명령어이다.  
`COPY`는 로컬 파일 또는 디렉토리를 컨테이너에 복사하는 기능만 지원한다.  
`ADD`도 해당 기능을 지원하지만, 2개의 또다른 기능도 지원한다.  
먼저, 로컬 파일 또는 디렉토리 대신 URL을 사용할 수 있다.  
두번째로는 ADD rootfs.tar.xz /처럼 소스에서 대상으로 직접 tar파일을 자동 추출할 수 있다.

```bash
// ADD, COPY 정의
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.5
FROM ubuntu
ADD  host.html  /test_dir/add.html
COPY host.html  /test_dir/copy.html
ADD  https://github.com/kubernetes/kubernetes/blob/master/LICENSE /test_dir/
ADD  add_data.tgz /test_dir/
COPY copy_data.tgz /test_dir/
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#

// 압축 파일 해제
root@docker1:~/labfile/dockerfile_dir# tar -tzvf add_data.tgz
-rw-r--r-- root/root        20 2022-04-26 12:08 add_1.html
-rw-r--r-- root/root        20 2022-04-26 12:08 add_2.html
-rw-r--r-- root/root        20 2022-04-26 12:09 add_3.html
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# tar -tzvf copy_data.tgz
-rw-r--r-- root/root        20 2022-04-26 12:09 copy_1.html
-rw-r--r-- root/root        20 2022-04-26 12:09 copy_2.html
-rw-r--r-- root/root        20 2022-04-26 12:09 copy_3.html
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# ls
add_data.tgz   Dockerfile    Dockerfile.11  Dockerfile.3  Dockerfile.5  host.html
copy_data.tgz  Dockerfile.1  Dockerfile.2   Dockerfile.4  Dockerfile.6  index.html
root@docker1:~/labfile/dockerfile_dir#

// file00 이미지 생성
root@docker1:~/labfile/dockerfile_dir# docker build -t file00 -f Dockerfile.5 .
Sending build context to Docker daemon  176.1kB
Step 1/6 : FROM ubuntu
latest: Pulling from library/ubuntu
6e3729cf69e0: Pull complete
Digest: sha256:27cb6e6ccef575a4698b66f5de06c7ecd61589132d5a91d098f7f3f9285415a9
Status: Downloaded newer image for ubuntu:latest
 ---> 6b7dfa7e8fdb
Step 2/6 : ADD  host.html  /test_dir/add.html
 ---> a9ad95733e3c
Step 3/6 : COPY host.html  /test_dir/copy.html
 ---> f5909d2eaad6
Step 4/6 : ADD  https://github.com/kubernetes/kubernetes/blob/master/LICENSE /test_dir/
Downloading  210.3kB

 ---> 48bbcf3a8eb9
Step 5/6 : ADD  add_data.tgz /test_dir/
 ---> b19bf7d0d6c5
Step 6/6 : COPY copy_data.tgz /test_dir/
 ---> 5a0fa506014b
Successfully built 5a0fa506014b
Successfully tagged file00:latest
root@docker1:~/labfile/dockerfile_dir# docker build -t file00 -f Dockerfile.5 .^C

// file00 이미지로 컨테이너 생성
root@docker1:~/labfile/dockerfile_dir# docker run -it --rm file00

// 컨테이너 내 COPY, ADD 파일 존재 확인
root@aa9154bcce3c:/# ls -l /test_dir/
total 544
-rw------- 1 root root 210306 Jan  1  1970 LICENSE
-rw-r--r-- 1 root root 163112 Jan 10 14:09 add.html
-rw-r--r-- 1 root root     20 Apr 26  2022 add_1.html
-rw-r--r-- 1 root root     20 Apr 26  2022 add_2.html
-rw-r--r-- 1 root root     20 Apr 26  2022 add_3.html
-rw-r--r-- 1 root root 163112 Jan 10 14:09 copy.html
-rw-r--r-- 1 root root    182 Jan 10 14:09 copy_data.tgz
```

### EXPOSE

컨테이너의 `공개 포트` 를 지정하여 도커에게 컨테이너가 대기하고 있는 포트를 알려준다.  
`docker run` 명령어의 `-p` 옵션을 통해 호스트의 포트 번호와 매핑할 수 있다.

```bash
// EXPOSE 선언
root@docker1:~/labfile/dockerfile_dir# cat Dockerfile.6
FROM nginx
EXPOSE 443
root@docker1:~/labfile/dockerfile_dir#

// 이미지 빌드
root@docker1:~/labfile/dockerfile_dir# docker build -t port00 -f Dockerfile.6 .
Sending build context to Docker daemon  186.9kB
Step 1/2 : FROM nginx
latest: Pulling from library/nginx
.. 중략
Successfully tagged port00:latest
root@docker1:~/labfile/dockerfile_dir#

// 컨테이너 생성
root@docker1:~/labfile/dockerfile_dir# docker run -d -p 8080:443 port00
878484cba54d9b332ad27177f5739bb3263018118ed357ea0e7a9627a15ea110
root@docker1:~/labfile/dockerfile_dir#

// 8080->443 확인
root@docker1:~/labfile/dockerfile_dir# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                                NAMES
878484cba54d   port00    "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   80/tcp, 0.0.0   .0:8080->443/tcp, :::8080->443/tcp   mystifying_merkle
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                     NAMES
878484cba54d   port00    "/docker-entrypoint.…"   17 seconds ago   Up 15 seconds   80/tcp, 0.0.0.->443/tcp   mystifying_merkle
root@docker1:~/labfile/dockerfile_dir# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                             NAMES
878484cba54d   port00    "/docker-entrypoint.…"   23 seconds ago   Up 22 seconds   80/tcp, 0.0.0.0:8080->443/tcp, :::8080->443/tcp   mystifying_merkle
root@docker1:~/labfile/dockerfile_dir#
```
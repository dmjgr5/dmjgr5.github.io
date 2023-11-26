---
layout: default
title: Docker Command
parent: Docker
grand_parent: Build & Package
permalink: /docs/buildpackage/docker-command/
nav_order: 23
---

# Docker Command
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
도커 컨테이너 실행을 위한 개념과 관련 명령어를 정리한다.



## `docker container ps -a`

컨테이너는 프로세스이므로 `ps` 로 조회한다.

> 실시간 상태 조회 : `watch -n 1 docker container ps -a`

## `docker image ls -a`

이미지는 파일이므로 `ls` 로 조회한다.

```bash
root@docker1:~# docker image ls -a
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
nginx        latest    1403e55ab369   2 weeks ago     142MB
centos       latest    5d0da3dc9764   16 months ago   231MB
root@docker1:~#
```


## `docker image inspect`

이미지의 상세정보를 출력한다.

```bash
root@docker1:~# docker image inspect centos
[
    {
        "Id": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "RepoTags": [
            "centos:latest"
        ],
        "RepoDigests": [
            "centos@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-09-15T18:20:05.184694267Z",
        "Container": "9bf8a9e2ddff4c0d76a587c40239679f29c863a967f23abf7a5babb6c2121bf1",
        "ContainerConfig": {
            "Hostname": "9bf8a9e2ddff",
            "Domainname": "",
```

## `docker image rm`

호스트에서 이미지를 삭제한다.

```bash
root@docker1:~# docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
nginx        latest    1403e55ab369   2 weeks ago     142MB
centos       latest    5d0da3dc9764   16 months ago   231MB
root@docker1:~#
root@docker1:~#
root@docker1:~# docker image rm centos
Untagged: centos:latest
Untagged: centos@sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Deleted: sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6
Deleted: sha256:74ddd0ec08fa43d09f32636ba91a0a3053b02cb4627c35051aff89f853606b59
root@docker1:~#
root@docker1:~# docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    1403e55ab369   2 weeks ago   142MB
root@docker1:~#

// 사용하지 않는 이미지 일괄 삭제
root@docker1:~# docker image prune -a
WARNING! This will remove all images without at least one container associated to them.
Are you sure you want to continue? [y/N] y
```


## `docker login`

도커 레지스트리에 업로드를 하려면 docker login 명령을 통해 로그인해야 한다.

```bash
// 로그인
root@docker1:~# docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: testuser
Password:
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
root@docker1:~#
root@docker1:~# cat /root/.docker/config.json
{
        "auths": {
                "https://index.docker.io/v1/": {
                        "auth": "ABCDEFJXXXXXXjg1M0Bzag=="
                }
        }
}root@docker1:~#
root@docker1:~#

// 로그 아웃
root@docker1:~# docker logout
Removing login credentials for https://index.docker.io/v1/
```




## `docker image pull`

Docker Hub 등의 Docker Registry 로 부터 Docker 이미지를 다운로드한다.

```bash
root@docker1:~# docker image pull docker.io/library/busybox:latest
latest: Pulling from library/busybox
205dae5015e7: Pull complete
Digest: sha256:7b3ccabffc97de872a30dfd234fd972a66d247c8cfc69b0550f276481852627c
Status: Downloaded newer image for busybox:latest
docker.io/library/busybox:latest

```

## `docker image tag`



이미지를 구분하기 위한 `tag` 를 새롭게 붙일 때 사용한다.

```bash
root@docker1:~# docker image ls -a
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
busybox      latest    66ba00ad3de8   5 days ago    4.87MB
nginx        latest    1403e55ab369   2 weeks ago   142MB
root@docker1:~#

// busybox 다운로드
root@docker1:~# docker image pull busybox
Using default tag: latest
latest: Pulling from library/busybox
Digest: sha256:7b3ccabffc97de872a30dfd234fd972a66d247c8cfc69b0550f276481852627c
Status: Image is up to date for busybox:latest
docker.io/library/busybox:latest
root@docker1:~#

// testuser/box 의 1.0 으로 생성
root@docker1:~# docker image tag busybox testuser/box:1.0
root@docker1:~#

// nginx 다운로드
root@docker1:~# docker image pull nginx
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:0047b729188a15da49380d9506d65959cce6d40291ccfb4e039f5dc7efd33286
Status: Image is up to date for nginx:latest
docker.io/library/nginx:latest
root@docker1:~#

// testuser/web-server 1.0 태그로 생성
root@docker1:~# docker image tag nginx testuser/web-server:1.0
root@docker1:~#

// 생성됐는지 조회
// 원래 이미지와 생성한 이미지의 IMAGE ID 가 동일하다.
root@docker1:~# docker image ls -a
REPOSITORY          TAG       IMAGE ID       CREATED       SIZE
testuser/box          1.0       66ba00ad3de8   5 days ago    4.87MB
busybox             latest    66ba00ad3de8   5 days ago    4.87MB
nginx               latest    1403e55ab369   2 weeks ago   142MB
testuser/web-server   1.0       1403e55ab369   2 weeks ago   142MB
root@docker1:~#
```

> Docker 레지스트리로부터 `pull` 할 때는 로그인이 필요없으나 `push` 할 때는 로그인이 필요하다.

```bash
root@docker1:~# docker image push testuser/web-server:1.0
The push refers to repository [docker.io/testuser/web-server]
c72d75f45e5b: Mounted from library/nginx
9a0ef04f57f5: Mounted from library/nginx
d13aea24d2cb: Mounted from library/nginx
2b3eec357807: Mounted from library/nginx
2dadbc36c170: Mounted from library/nginx
8a70d251b653: Mounted from library/nginx
1.0: digest: sha256:9a821cadb1b13cb782ec66445325045b2213459008a41c72d8d87cde94b33c8c size: 1570
root@docker1:~#
```

도커 이미지가 아래와 같이 PUSH 된 것을 확인할 수 있다.
 
![](/assets/images/dockercommand1.png)



호스트에서 지운 후 Pull 하면 이미지가 다운로드 된 것을 확인할 수 있다.

```bash
root@docker1:~# docker image rm testuser/web-server:1.0
Untagged: testuser/web-server:1.0
Untagged: testuser/web-server@sha256:9a821cadb1b13cb782ec66445325045b2213459008a41c72d8d87cde94b33c8c
root@docker1:~#
root@docker1:~# docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
testuser/box   1.0       66ba00ad3de8   5 days ago    4.87MB
busybox      latest    66ba00ad3de8   5 days ago    4.87MB
nginx        latest    1403e55ab369   2 weeks ago   142MB
root@docker1:~#
root@docker1:~# docker image pull testuser/web-server:1.0
1.0: Pulling from testuser/web-server
Digest: sha256:9a821cadb1b13cb782ec66445325045b2213459008a41c72d8d87cde94b33c8c
Status: Downloaded newer image for testuser/web-server:1.0
docker.io/testuser/web-server:1.0
root@docker1:~#
root@docker1:~# docker image ls
REPOSITORY          TAG       IMAGE ID       CREATED       SIZE
testuser/box          1.0       66ba00ad3de8   5 days ago    4.87MB
busybox             latest    66ba00ad3de8   5 days ago    4.87MB
testuser/web-server   1.0       1403e55ab369   2 weeks ago   142MB
nginx               latest    1403e55ab369   2 weeks ago   142MB
root@docker1:~#
```

## `docker system prune`

더 이상 사용하지 않는 이미지/컨테이너/네트워크를 일괄 삭제한다.

## `docker container commit`

이미지 생성에는 두가지 방법이 있다.

-   `docker container commit` 와 같이 컨테이너 상태를 기반으로 이미지를 생성하는 방법
-   `Dockerfile` 을 빌드(Build) 해서 생성하는 방법이 있다. 후자가 많이 쓰이는 방법이다.
 


![](/assets/images/dockercommand2.png)

*[도커 이미지 생성의 2가지 방법 ]*
{: .text-center }



```bash
// ubuntu 를  sample 컨테이너로 만든다.
root@docker1:~# docker run -it --name sample ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
6e3729cf69e0: Pull complete
Digest: sha256:27cb6e6ccef575a4698b66f5de06c7ecd61589132d5a91d098f7f3f9285415a9
Status: Downloaded newer image for ubuntu:latest
root@8a80c4cde61e:/# apt-get update
Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
 ...
Get:18 http://archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [3520 B]
Fetched 25.1 MB in 1min 38s (257 kB/s)
Reading package lists... Done
root@8a80c4cde61e:/#

// 컨테이너에서 nginx 를 설치한다.
root@8a80c4cde61e:/# apt-get install -y nginx
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  fontconfig-config fonts-dejavu-core iproute2 libatm1 libbpf0 libbrotli1 libbsd0 libcap2-bin
  libdeflate0 libelf1 libexpat1 libfontconfig1 libfreetype6 libgd3 libicu70 libjbig0 libjpeg-turbo8
  libjpeg8 libmaxminddb0 libmd0 libmnl0 libnginx-mod-http-geoip2 libnginx-mod-http-image-filter
  libnginx-mod-http-xslt-filter libnginx-mod-mail libnginx-mod-stream libnginx-mod-stream-geoip2
  libpam-cap libpng16-16 libtiff5 libwebp7 libx11-6 libx11-data libxau6 libxcb1 libxdmcp6 libxml2
  libxpm4 libxslt1.1 libxtables12 nginx-common nginx-core ucf
Suggested packages:
Setting up nginx (1.18.0-6ubuntu14.3) ...
Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
root@8a80c4cde61e:/# CTRL+PQ

// detach 모드로 전환

root@docker1:~#
root@docker1:~# echo Hello world  > index.html
root@docker1:~# docker container cp index.html   sample:/var/www/html/index.html

// index.html 수정 후 sample 컨테이너를 testuser/web-nginx:1.0 이미지 생성
root@docker1:~# docker container commit -a "TESTUSER" -m "Customized image" sample testuser/web-nginx:1.0
sha256:6f10cf278ad2f4ba4017850c0ced7b77230671e613773bc46dd7822cbcccaba1
root@docker1:~#

// testuser/web-nginx    1.0 이미지 생성 확인
root@docker1:~# docker image ls
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
testuser/web-nginx    1.0       6f10cf278ad2   8 seconds ago   174MB
busybox             latest    66ba00ad3de8   5 days ago      4.87MB
testuser/box          1.0       66ba00ad3de8   5 days ago      4.87MB
testuser/web-server   1.0       1403e55ab369   2 weeks ago     142MB
nginx               latest    1403e55ab369   2 weeks ago     142MB
ubuntu              latest    6b7dfa7e8fdb   4 weeks ago     77.8MB
root@docker1:~#

// 이미지 상세 확인 (Comment, Author)
root@docker1:~# docker image inspect testuser/web-nginx:1.0
[
    {
        "Id": "sha256:6f10cf278ad2f4ba4017850c0ced7b77230671e613773bc46dd7822cbcccaba1",
        "RepoTags": [
            "testuser/web-nginx:1.0"
        "Comment": "Customized image",
        "Created": "2023-01-09T14:06:44.617231726Z",
        "DockerVersion": "20.10.22",
        "Author": "TESTUSER",

```

[##_Image|kage@da6tcx/btsjkiihTDb/zgfvyFqTXkcOQVUDunVck0/img.png|CDM|1.3|{"originWidth":881,"originHeight":130,"style":"alignCenter","caption":"도커 이미지 Push 상태 확인","filename":"edited_blob"}_##]

```bash

 // 도커이미지에서 testuser/web-nginx:1.0 를 가져와 컨테이너를 생성한다.

 root@docker1:~# docker container run -d --name web -p 8080:80 testuser/web-nginx:1.0  nginx -g 'daemon off; '
Unable to find image 'testuser/web-nginx:1.0' locally
1.0: Pulling from testuser/web-nginx
6e3729cf69e0: Already exists
7c9aaa2fac99: Pull complete
Digest: sha256:4150994823e51b64caa7b5ccdbb543703e776d519596562b02a9a127ffe770c0
Status: Downloaded newer image for testuser/web-nginx:1.0
dd4e6613c4cf4ada677b19c67cfb2656d2849d72410850a80a0d6c76cc4f35bb


// 가져온 이미지가 있다!!

root@docker1:~# docker image ls
REPOSITORY          TAG       IMAGE ID       CREATED          SIZE
testuser/web-nginx    1.0       6f10cf278ad2   23 minutes ago   174MB
testuser/box          1.0       66ba00ad3de8   5 days ago       4.87MB
busybox             latest    66ba00ad3de8   5 days ago       4.87MB
testuser/web-server   1.0       1403e55ab369   2 weeks ago      142MB
nginx               latest    1403e55ab369   2 weeks ago      142MB
ubuntu              latest    6b7dfa7e8fdb   4 weeks ago      77.8MB

```

`web` 이란 이름의 컨테이너 생성확인

```bash
Every 1.0s: docker container ps -a                                                                                                docker1: Mon Jan  9 14:23:23 2023

CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                   NAMES
dd4e6613c4cf   testuser/web-nginx:1.0   "nginx -g 'daemon of…"   36 seconds ago   Up 35 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   web
8a80c4cde61e   ubuntu                 "bash"                   20 minutes ago   Up 20 minutes                                           sample
```

`localhost:8080` 접속 확인

[##_Image|kage@bzUgM6/btsjkFYTbkU/8AzXGVzjQqZ50V21OfSdKK/img.png|CDM|1.3|{"originWidth":675,"originHeight":181,"style":"alignLeft"}_##]

## `docker image history`

이미지를 생성할 때 어떤 명령이 실행됐는지 확인

```bash
root@docker1:~/labfile/dockerfile_dir# docker image history file00
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
5a0fa506014b   24 minutes ago   /bin/sh -c #(nop) COPY file:d1f4434c224263e4…   182B
b19bf7d0d6c5   24 minutes ago   /bin/sh -c #(nop) ADD file:93679052316aa8731…   60B
48bbcf3a8eb9   24 minutes ago   /bin/sh -c #(nop) ADD 622787ee69d99fd65381b7…   210kB
f5909d2eaad6   24 minutes ago   /bin/sh -c #(nop) COPY file:c849731474b116b7…   163kB
a9ad95733e3c   24 minutes ago   /bin/sh -c #(nop) ADD file:c849731474b116b74…   163kB
6b7dfa7e8fdb   4 weeks ago      /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>      4 weeks ago      /bin/sh -c #(nop) ADD file:481dd2da6de715252…   77.8MB
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# docker image history file00 --no-trunc
IMAGE                                                                     CREATED          CREATE                                     D BY                                                                                                                                                  SIZE      COMMENT
sha256:5a0fa506014b34bacab3717be24c1351aff9ecab9e2368d9d1e3e7b7347ded77   24 minutes ago   /bin/s                                     h -c #(nop) COPY file:d1f4434c224263e494ca1d96889c7976f80fd60648af4bdcd9e8dcae32cbc46c in /test_d                                     ir/             182B
sha256:b19bf7d0d6c5ff52dc1086f5f58e9390bbe72c1c6f2e77d208bfd40321ddcebd   24 minutes ago   /bin/s                                     h -c #(nop) ADD file:93679052316aa8731d3a54bb37cece254064b24078712f99a20746f969b526cb in /test_di                                     r/              60B
sha256:48bbcf3a8eb99f0536f15ff731e412ae7aec8c94d4f411a81403cc3d0844f3b5   24 minutes ago   /bin/s                                     h -c #(nop) ADD 622787ee69d99fd65381b70cc87bbe47bdbe82213236bc5e33db771e6fa11228 in /test_dir/                                                        210kB
sha256:f5909d2eaad6bf5abf0693a7b00a034c091ae7120b58f0cce23f35faafdd560d   24 minutes ago   /bin/s                                     h -c #(nop) COPY file:c849731474b116b7485f4172a68fe1206ec51f0a405cfbf72a11d4eb3fb81fc6 in /test_d                                     ir/copy.html    163kB
sha256:a9ad95733e3c3243ea1cc62669a88fb9e48defc9012039c385f49e503c84ad82   24 minutes ago   /bin/s                                     h -c #(nop) ADD file:c849731474b116b7485f4172a68fe1206ec51f0a405cfbf72a11d4eb3fb81fc6 in /test_di                                     r/add.html      163kB
sha256:6b7dfa7e8fdbe18ad425dd965a1049d984f31cf0ad57fa6d5377cca355e65f03   4 weeks ago      /bin/s                                     h -c #(nop)  CMD ["bash"]                                                                                                                             0B
<missing>                                                                 4 weeks ago      /bin/s                                     h -c #(nop) ADD file:481dd2da6de71525248eba186feeeafcc73cc956ade0a196a4e8b0c2424e74b9 in /                                                            77.8MB
```

> \--no-trunc : 전체 문자열 넣기

## `docker container create`

컨테이너를 생성하고 자동으로 시작하지는 않는다.

```bash
root@docker1:~# docker container ps -a <-- 컨테이너는 프로세스이므로 ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
root@docker1:~# docker image ls -a <-- 이미지는 파일이므로 ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
root@docker1:~#

root@docker1:~# docker container create -it --name testos centos
unknown flag: --names
See 'docker container create --help'.
root@docker1:~# docker container create -it --name testos centos
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
d2acdab7c45dc1cc2d273839f607bb2b24dfca153714360eb532a0237c56d093
root@docker1:~#

root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS    PORTS     NAMES
d2acdab7c45d   centos    "/bin/bash"   6 seconds ago   Created             testos
root@docker1:~# docker image ls -a
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
centos       latest    5d0da3dc9764   15 months ago   231MB
root@docker1:~#
```

> `docker container create -it --name testos centos`  
> 옵션 -i : `interactive` 의미로 표준 입출력을 받을 수 있도록 한다. 컨테이너 내에 shell 같은 기능이 있을 경우 사용한다.  
> 옵션 -t : `tty` 의미로 컨테이너의 가상 터미널을 의미한다.  
> `--name testos` : `testos` 라는 이름의 컨테이너명을 지정한다.  
> `centos` : 로컬 레포지토리에 이미지가 없으면, 기본은 `docker image` 에서 이미지를 pull 한다. 한 번 pull 한 이미지는 재사용이 가능하다.

## `docker container start`

컨테이너를 시작한다.

```bash
root@docker1:~# docker container start -ai testos <-- 기존 생성했던 컨테이너

[root@d2acdab7c45d /]# ps -e <-- 컨테이너 내부로 들어옴.
  PID TTY          TIME CMD
    1 pts/0    00:00:00 bash
   14 pts/0    00:00:00 ps
[root@d2acdab7c45d /]# <-- ctrl+P+Q 단축키로 detach 모드로 전환(컨테이너 살아있음, exit 명령을 사용하면 bin/bash 종료되면서 컨테이너도 함께 종료된다.
[root@d2acdab7c45d /]# root@docker1:~#
root@docker1:~#

root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS              PORTS     NAMES
d2acdab7c45d   centos    "/bin/bash"   14 minutes ago   Up About a minute             testos
root@docker1:~#
```

> 옵션 -a : `attach` 의 의미로 해당 컨테이너 내부로 접근하는 것을 의미한다

## `docker container stop`

컨테이너를 종료한다.

```bash
root@docker1:~# docker container stop testos
testos
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                     PORTS     NAMES
d2acdab7c45d   centos    "/bin/bash"   18 minutes ago   Exited (0) 3 seconds ago             testos
root@docker1:~#
```

## `docker container run`

컨테이너를 시작하고(없으면 생성), COMMAND(PID=1 프로세스, 도커 엔진에 의해서 격리될 프로세스) 를 실행한다.

```bash
root@docker1:~# docker container run -it --name testos centos

[root@2b87afc91519 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 06:39 pts/0    00:00:00 /bin/bash
root        17     1  0 06:40 pts/0    00:00:00 ps -ef

[root@2b87afc91519 /]# hostname
2b87afc91519
```

Detach 모드로 전환 후 생성한 컨테이너 조회한다.

```bash
[root@2b87afc91519 /]# root@docker1:~# 
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
2b87afc91519   centos    "/bin/bash"   4 minutes ago   Up 4 minutes             testos
root@docker1:~#
```

컨테이너를 생성 이후 시작할 때 COMMAND 를 임의로 정의할 수 있다.

```bash
root@docker1:~# docker container run -it --name testos1 centos /bin/date
Sun Jan  8 06:46:12 UTC 2023
root@docker1:~#

root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                      PORTS     NAMES
cfd15085385b   centos    "/bin/date"   38 seconds ago   Exited (0) 37 seconds ago             testos1
2b87afc91519   centos    "/bin/bash"   7 minutes ago    Up 7 minutes                          testos
root@docker1:~#
```

> 여기서 `/bin/date` 는 시작 후 바로 종료되는 프로세스이므로 해당 컨테이너는 종료된다. 다른 예로서 AI 계산에 이용되는 컨테이너가 될 수 있다.

`-d` (`--detach`) : 컨테이너 PID 1 번 프로세스를 백그라운드로 실행하기 위해서는 `-d` 옵션을 사용한다.

```bash
root@docker1:~# docker container run -d --name testos2 centos /bin/ping localhost
49301e70f50e8a954ec3974a5cfbd5fecee85ded760b38abbfeff385681acfdf
root@docker1:~#

root@docker1:~# docker container logs testos2 <-- 로그 파일이 아닌 1번의 출력 내용
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.072 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.054 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.051 ms
64 bytes from localhost (127.0.0.1): icmp_seq=4 ttl=64 time=0.064 ms
64 bytes from localhost (127.0.0.1): icmp_seq=5 ttl=64 time=0.102 ms
64 bytes from localhost (127.0.0.1): icmp_seq=6 ttl=64 time=0.053 ms
root@docker1:~#

root@docker1:~# docker container exec -it testos2 /bin/bash <-- exec: 1번 프로세스 외의 추가 프로그램 실행
[root@49301e70f50e /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 06:58 ?        00:00:00 /bin/ping localhost
root         6     0  0 06:59 pts/0    00:00:00 /bin/bash
root        19     6  0 06:59 pts/0    00:00:00 ps -ef
[root@49301e70f50e /]#
[root@49301e70f50e /]# read escape sequence
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND                 CREATED              STATUS                      PORTS                     NAMES
49301e70f50e   centos    "/bin/ping localhost"   About a minute ago   Up About a minute                                     testos2
cfd15085385b   centos    "/bin/date"             13 minutes ago       Exited (0) 13 minutes ago                             testos1
2b87afc91519   centos    "/bin/bash"             20 minutes ago       Up 20 minutes                                         testos
```

## `docker container attach`

컨테이너 내부에 접근하여 PID 1 번 프로세스에 접근한다.

```bash
root@docker1:~# docker container run -it --name testos3 centos

[root@f1817a32d216 /]# root@docker1:~# <-- detach 모드로 변경
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND                 CREATED          STATUS                      PORTS     NAMES
f1817a32d216   centos    "/bin/bash"             17 seconds ago   Up 16 seconds                         testos3
49301e70f50e   centos    "/bin/ping localhost"   7 minutes ago    Up 7 minutes                          testos2
cfd15085385b   centos    "/bin/date"             19 minutes ago   Exited (0) 19 minutes ago             testos1
2b87afc91519   centos    "/bin/bash"             26 minutes ago   Up 26 minutes                         testos
root@docker1:~#

root@docker1:~# docker container attach testos3 <-- testos3 의 컨테이너로 접근
[root@f1817a32d216 /]# ps -e
  PID TTY          TIME CMD
    1 pts/0    00:00:00 bash
   14 pts/0    00:00:00 ps
[root@f1817a32d216 /]#
```

## `docker container cp`

컨테이너와 호스트 간의 파일을 복사할 수 있따.

```bash
root@docker1:~# docker container run -itd --name=testos5 centos
8133bc90c42bb60337c4c43bacac0dc081c3994710b2973d45b5fbd81283103f
root@docker1:~#
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND                 CREATED          STATUS                      PORTS     NAMES
8133bc90c42b   centos    "/bin/bash"             8 seconds ago    Up 7 seconds                          testos5
76cd9ce3f900   centos    "/bin/bash"             2 minutes ago    Exited (0) 2 minutes ago              testos4
f1817a32d216   centos    "/bin/bash"             5 minutes ago    Up 5 minutes                          testos3
49301e70f50e   centos    "/bin/ping localhost"   12 minutes ago   Up 12 minutes                         testos2
cfd15085385b   centos    "/bin/date"             24 minutes ago   Exited (0) 24 minutes ago             testos1
2b87afc91519   centos    "/bin/bash"             31 minutes ago   Up 31 minutes                         testos
root@docker1:~#

root@docker1:~# echo "hello world.." > hostfile <-- 호스트에서 파일 생성
root@docker1:~#
root@docker1:~# docker container cp hostfile testos5:/containerfile <-- 호스트에서 컨테이너로 복사
root@docker1:~#
root@docker1:~# docker container exec testos5 ls -l /containerfile <-- 컨테이너에 접근 및 파일 조회
-rw-r--r-- 1 root root 14 Jan  8 07:11 /containerfile
root@docker1:~#
root@docker1:~#

root@docker1:~# ls -l containerfile
ls: cannot access 'containerfile': No such file or directory
root@docker1:~#
root@docker1:~# docker container cp testos5:/containerfile containerfile  <-- 컨테이너에서 호스트로 파일 복사
root@docker1:~# ls -l containerfile <-- 호스트에 파일 조회
-rw-r--r-- 1 root root 14 Jan  8 07:11 containerfile

```

## `docker container rm`

컨테이너 삭제할 수 있으며 강제 삭제의 경우에는 `-f` 옵션을 추가한다.

```bash
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND                 CREATED          STATUS                      PORTS     NAMES
8133bc90c42b   centos    "/bin/bash"             7 minutes ago    Up 7 minutes                          testos5
76cd9ce3f900   centos    "/bin/bash"             10 minutes ago   Exited (0) 10 minutes ago             testos4
f1817a32d216   centos    "/bin/bash"             12 minutes ago   Up 12 minutes                         testos3
49301e70f50e   centos    "/bin/ping localhost"   20 minutes ago   Up 20 minutes                         testos2
cfd15085385b   centos    "/bin/date"             32 minutes ago   Exited (0) 32 minutes ago             testos1
2b87afc91519   centos    "/bin/bash"             39 minutes ago   Up 39 minutes                         testos
root@docker1:~#

root@docker1:~# docker container rm testos5 <-- 삭제
Error response from daemon: You cannot remove a running container 8133bc90c42bb60337c4c43bacac0dc081c3994710b2973d45b5fbd81283103f. Stop the container before attempting removal or force remove
root@docker1:~#

root@docker1:~# docker container rm -f testos5 <-- 강제 삭제
testos5
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND                 CREATED          STATUS                      PORTS     NAMES
76cd9ce3f900   centos    "/bin/bash"             10 minutes ago   Exited (0) 10 minutes ago             testos4
f1817a32d216   centos    "/bin/bash"             13 minutes ago   Up 13 minutes                         testos3
49301e70f50e   centos    "/bin/ping localhost"   21 minutes ago   Up 21 minutes                         testos2
cfd15085385b   centos    "/bin/date"             33 minutes ago   Exited (0) 33 minutes ago             testos1
2b87afc91519   centos    "/bin/bash"             39 minutes ago   Up 39 minutes                         testos
root@docker1:~#
```

-   컨테이너 상태 실시간 조회

```bash
watch -n 1 docker container ps -a 

Every 1.0s: docker container ps -a                                                      docker1: Sun Jan  8 07:31:20 2023

CONTAINER ID   IMAGE     COMMAND                 CREATED          STATUS                      PORTS     NAMES
76cd9ce3f900   centos    "/bin/bash"             22 minutes ago   Exited (0) 8 minutes ago              testos4
f1817a32d216   centos    "/bin/bash"             25 minutes ago   Up 25 minutes                         testos3
49301e70f50e   centos    "/bin/ping localhost"   33 minutes ago   Up 33 minutes                         testos2
cfd15085385b   centos    "/bin/date"             45 minutes ago   Exited (0) 45 minutes ago             testos1
2b87afc91519   centos    "/bin/bash"             51 minutes ago   Up 51 minutes     
```

-   이미 종료된 컨테이너들을 일괄 삭제

```bash
root@docker1:~# docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
76cd9ce3f9004b782084e9f0d54521e6db482f38a2d4d5dcde558a3bac19c789
cfd15085385b656c48a35c4f634984524bf98b406ff6f33d10a33b3d5b852f41

Total reclaimed space: 0B
root@docker1:~#
root@docker1:~#
```

> `# docker container rm -f $(docker container ps -aq)` : up, exited 모든 컨테이너를 일괄삭제

## `docker container diff`

컨테이너 생성 시점부터 변경된 이력(파일 또는 디렉토리) 을 확인한다.

```bash
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND                 CREATED          STATUS                      PORTS     NAMES
76cd9ce3f900   centos    "/bin/bash"             14 minutes ago   Exited (0) 3 seconds ago              testos4
f1817a32d216   centos    "/bin/bash"             17 minutes ago   Up 17 minutes                         testos3
49301e70f50e   centos    "/bin/ping localhost"   24 minutes ago   Up 24 minutes                         testos2
cfd15085385b   centos    "/bin/date"             36 minutes ago   Exited (0) 36 minutes ago             testos1
2b87afc91519   centos    "/bin/bash"             43 minutes ago   Up 43 minutes                         testos

root@docker1:~# docker container attach testos3 <-- testos3 에 접근
[root@f1817a32d216 /]#

[root@f1817a32d216 /]# useradd containeruser <-- user 추가
[root@f1817a32d216 /]# read escape sequence

root@docker1:~# docker container diff testos3 <-- 이력 조회
C /var
C /var/log
C /var/log/lastlog
C /var/spool
C /var/spool/mail
A /var/spool/mail/containeruser
C /home
A /home/containeruser
A /home/containeruser/.bashrc
A /home/containeruser/.bash_logout
A /home/containeruser/.bash_profile
C /etc
C /etc/group
C /etc/gshadow-
C /etc/passwd
C /etc/shadow
C /etc/passwd-
C /etc/group-
A /etc/subgid-
C /etc/subgid
C /etc/subuid
C /etc/shadow-
A /etc/subuid-
C /etc/gshadow
root@docker1:~#
```

> `C` : Change, `A` : Add, `D` : Delete
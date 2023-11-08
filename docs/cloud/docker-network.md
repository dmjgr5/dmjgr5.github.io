---
layout: default
title: Docker Network
parent: Docker
grand_parent: Cloud
permalink: /docs/cloud/docker-network/
nav_order: 25
---

# Docker Network
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
컨테이너 내에서 생성 또는 변경된 모든 파일은 `Read-Write` 가능한 Container Layer 에 저장된다. 도커는 Copy-on-write (`CoW`) 방식으로 파일을 관리한다.  
컨테이너가 삭제되면, 데이터가 영구적으로 유지되지 않기 때문에 다른 프로세스나 컨테이너가 해당 데이터를 필요로 하는 경우 사용할 수 없게 된다. 이 때문에 `Volume` 즉, 영구적인 저장소가 필요하다.

 
## 도커 네트워크의 동작 원리

### Bridged Network (default)

도커는 컨테이너에 내부 IP ( default : `172.17.0.0/18`) 을 순차적으로 할당하며, 할당된 IP 는 재시작할 때마다 변경될 수 있다. 이 내부 IP 는 도커가 설치된 호스트 내부에서만 사용될 수 있다.  
각 컨테이너가 외부와 네트워크 연결을 위해서는 도커가 자동으로 `veth` 로 시작하는 가상 네트워크 인터페이스를 호스트에 생성한다.
 
![](/assets/images/dockernetwork1.png)

`docker0` 브리지 인터페이스는 각 `veth` 인터페이스와 바인딩되어 호스트의 네트워크 인터페이스와 연결해주는 역할을 한다.
 
![](/assets/images/dockernetwork2.png)

### 가상 bridge 및 가상 NIC

  
![](/assets/images/dockernetwork3.png)

*[도커 네트워크 구조]*
{: .text-center }
  


컨테이너가 외부 네트워크와 통신할 때에는 가상 브리지(bridge) `docker0` 와 호스트의 물리 NIC 를 통해서 패킷을 전송한다.

-   NAPT(`Network Address Port Translation`)
-   하나의 IP Address 를 여러 컴퓨터에서 공유하는 기술로 IP Address 뿐 아니라 Port 까지 변환
-   Private IP 를 Public IP 로 상호 변환

## 주요 실습

`-p 8080:80` : publisher 호스트(수신할 때의 포트 번호) : 컨테이너(내부)

```bash

// w1 컨테이너 생성( 외부 포트 8080) 
root@docker1:~# docker container run -d --name w1 -p 8080:80 nginx
6e8da9ae773109d923f32750f217c4aa1eefde00c04ab878500f596eaeefe44c
root@docker1:~#

// w2 컨테이너 생성( 외부 포트 8081)
root@docker1:~# docker container run -d --name w2 -p 8181:80 nginx
2ee56918d0d5eaf3206f90ab88f6ad7419ab6d8751026c6deb4c7eedf22b1d85
root@docker1:~#
root@docker1:~#
root@docker1:~# docker container ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
2ee56918d0d5   nginx     "/docker-entrypoint.…"   2 seconds ago    Up 2 seconds    0.0.0.0:8181->80/tcp, :::8181->80/tcp   w2
6e8da9ae7731   nginx     "/docker-entrypoint.…"   56 seconds ago   Up 56 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   w1

// 컨테이너 IP 확인
root@docker1:~# docker container inspect w1  | grep IPAddr
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.2",
                    "IPAddress": "172.17.0.2",
root@docker1:~# docker container inspect w2 | grep IPAddr
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.3",
                    "IPAddress": "172.17.0.3",
root@docker1:~#
root@docker1:~#

// curl 조회
root@docker1:~# curl -sf http://172.17.0.2
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@docker1:~#
root@docker1:~#
root@docker1:~# curl -sf http://172.17.0.3
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@docker1:~#
root@docker1:~#

// index.html 파일 변경
root@docker1:~# echo Welcome to the w1 service > index.html
root@docker1:~# docker container cp index.html   w1:/usr/share/nginx/html/index.html
root@docker1:~#
root@docker1:~# echo Welcome to the w2 service > index.html
root@docker1:~# docker container cp index.html   w2:/usr/share/nginx/html/index.html
root@docker1:~#

// curl 조회
root@docker1:~# curl -sf http://172.17.0.2
Welcome to the w1 service
root@docker1:~#
root@docker1:~# curl -sf http://172.17.0.3
Welcome to the w2 service
root@docker1:~#

// 외부 환경에서의 조회
root@docker1:~# curl -sf http://192.168.137.101:8080
Welcome to the w1 service
root@docker1:~#
root@docker1:~# curl -sf http://192.168.137.101:8181
Welcome to the w2 service
root@docker1:~#

// iptables 조회 (Chain DOCKER)
root@docker1:~# iptables -t nat -vnL
Chain PREROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    3   132 DOCKER     all  --  *      *       0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL

Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 9 packets, 611 bytes)
 pkts bytes target     prot opt in     out     source               destination
    2   120 DOCKER     all  --  *      *       0.0.0.0/0           !127.0.0.0/8          ADDRTYPE match dst-type LOCAL

Chain POSTROUTING (policy ACCEPT 11 packets, 731 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 MASQUERADE  all  --  *      !docker0  172.17.0.0/16        0.0.0.0/0
    0     0 MASQUERADE  tcp  --  *      *       172.17.0.2           172.17.0.2           tcp dpt:80
    0     0 MASQUERADE  tcp  --  *      *       172.17.0.3           172.17.0.3           tcp dpt:80

Chain DOCKER (2 references)
 pkts bytes target     prot opt in     out     source               destination
    0     0 RETURN     all  --  docker0 *       0.0.0.0/0            0.0.0.0/0
    1    60 DNAT       tcp  --  !docker0 *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8080 to:172.17.0.2:80
    1    60 DNAT       tcp  --  !docker0 *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8181 to:172.17.0.3:80
```
 
```bash
// testos 컨테이너 생성
root@docker1:~# docker container run -itd --name=testos centos
6f415171cba166d01d5bcfb918d2c2e19829424960507e3dd7930ba3761e1718
root@docker1:~#

// grep 조회
root@docker1:~# docker container inspect testos | grep -C2 UpperDir
                "LowerDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af8                        1-init/diff:/var/lib/docker/overlay2/94606eea6540dbe5a3f3a03aa3fe4977ac7fcdf1aeba8e1a346cc88209df2bc5/diff",
                "MergedDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af                        81/merged",
                "UpperDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af8                        1/diff",
                "WorkDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81                        /work"
            },
root@docker1:~# docker container inspect testos | grep -C2 UpperDir
                "LowerDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81-init/diff:/var/lib/docker/overlay2/94606eea6540dbe5a3f3a03aa3fe4977ac7fcdf1aeba8e1a346cc88209df2bc5/diff",
                "MergedDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81/merged",
                "UpperDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81/diff",
                "WorkDir": "/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81/work"
            },
root@docker1:~#

//  호스트에서 UpperDir 경로 조회 및 파일 생성
root@docker1:~# cd /var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81/diff
root@docker1:/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81/diff# touch containerfile
root@docker1:/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81/diff# ll
total 8
drwxr-xr-x 2 root root 4096 Jan  8 12:57 ./
drwx--x--- 5 root root 4096 Jan  8 12:56 ../
-rw-r--r-- 1 root root    0 Jan  8 12:57 containerfile
root@docker1:/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f

// testos 컨테이너에서 파일 생성 확인
root@docker1:/var/lib/docker/overlay2/180352a45d8b916b4c541a2cbd908ff372e9bb4ab3e0c880b476f43fd429af81/diff# docker container exec testos /bin/ls -l containerfile
-rw-r--r-- 1 root root 0 Jan  8 12:57 containerfile
```

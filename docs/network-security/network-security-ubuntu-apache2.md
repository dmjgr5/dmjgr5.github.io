---
layout: default
title: Ubuntu 에서 apache2 설치하기
parent: Network & Security
nav_order: 10
---


# Ubuntu 에서 apache2 설치하기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

{: .note}
아파치를 설치하기 위해서는 CentOS 에서는 httpd 을 설치하는 반면, Ubuntu 에서는 apache2 를 설치한다. Ubuntu 에서 apache2 를 설치해본다.


## apache2 설치하기

아래 명령어로 apache2 를 install 한다.

```sh
root@user1-500R5K-501R5K-500R5Q:~# apt-get install apache2
```

## apache2 설치 확인하기

설치가 완료되면 아래와 같이 서비스 상태가 running 상태로 조회된다.

```sh
root@user1-500R5K-501R5K-500R5Q:~# systemctl status apache2.service 
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-12-22 21:20:11 KST; 26s ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 8164 (apache2)
      Tasks: 55 (limit: 9323)
     Memory: 5.2M
     CGroup: /system.slice/apache2.service
             ├─8164 /usr/sbin/apache2 -k start
             ├─8165 /usr/sbin/apache2 -k start
             └─8166 /usr/sbin/apache2 -k start

12월 22 21:20:11 user1-500R5K-501R5K-500R5Q systemd[1]: Starting The Apache HTTP Server...
12월 22 21:20:11 user1-500R5K-501R5K-500R5Q apachectl[8163]: AH00558: apache2: Could not reliably determine the >
12월 22 21:20:11 user1-500R5K-501R5K-500R5Q systemd[1]: Started The Apache HTTP Server.
```

또한 `/etc/init.d` 라는 데몬 프로그램을 관리하는 디렉토리에 `apache2` 가 설치된 것을 확인할 수 있다.

```sh
root@user1-500R5K-501R5K-500R5Q:~# ls -al /etc/init.d/
total 244
drwxr-xr-x   2 root root  4096 12월 22 21:20 .
drwxr-xr-x 150 root root 12288 12월 22 21:20 ..
-rwxr-xr-x   1 root root  2269 11월 28  2019 acpid
-rwxr-xr-x   1 root root  5574 11월  6  2019 alsa-utils
-rwxr-xr-x   1 root root  2055  7월 17  2019 anacron
-rwxr-xr-x   1 root root  8181  2월 24  2021 apache2
-rwxr-xr-x   1 root root  2489  2월 24  2021 apache-htcacheclean
-rwxr-xr-x   1 root root  3740  4월  1  2020 apparmor
-rwxr-xr-x   1 root root  2915  6월 29  2022 apport
```



## apache2 접속 확인하기

아래와 같이 localhost 로 브라우저에서 접속하면 기본 페이지를 확인할 수 있다.

![Alt text](/assets/images/apache2-1.png)


## apache2 서비스 종료, 재시작하기
 
```sh
root@user1-500R5K-501R5K-500R5Q:~# systemctl stop apache2.service 
root@user1-500R5K-501R5K-500R5Q:~# systemctl status  apache2.service 
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Fri 2023-12-22 21:30:44 KST; 6s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 9735 ExecStop=/usr/sbin/apachectl stop (code=exited, status=0/SUCCESS)
   Main PID: 8164 (code=exited, status=0/SUCCESS)

12월 22 21:20:11 user1-500R5K-501R5K-500R5Q systemd[1]: Starting The Apache HTTP Server...
12월 22 21:20:11 user1-500R5K-501R5K-500R5Q apachectl[8163]: AH00558: apache2: Could not reliably determine the >
12월 22 21:20:11 user1-500R5K-501R5K-500R5Q systemd[1]: Started The Apache HTTP Server.
12월 22 21:30:44 user1-500R5K-501R5K-500R5Q systemd[1]: Stopping The Apache HTTP Server...
12월 22 21:30:44 user1-500R5K-501R5K-500R5Q apachectl[9737]: AH00558: apache2: Could not reliably determine the >
12월 22 21:30:44 user1-500R5K-501R5K-500R5Q systemd[1]: apache2.service: Succeeded.
12월 22 21:30:44 user1-500R5K-501R5K-500R5Q systemd[1]: Stopped The Apache HTTP Server.

```

```sh
root@user1-500R5K-501R5K-500R5Q:~# systemctl restart apache2.service 
root@user1-500R5K-501R5K-500R5Q:~# systemctl status  apache2.service 
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2023-12-22 21:31:47 KST; 3s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 9805 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SUCCESS)
   Main PID: 9809 (apache2)
      Tasks: 55 (limit: 9323)
     Memory: 5.0M
     CGroup: /system.slice/apache2.service
             ├─9809 /usr/sbin/apache2 -k start
             ├─9810 /usr/sbin/apache2 -k start
             └─9811 /usr/sbin/apache2 -k start

12월 22 21:31:47 user1-500R5K-501R5K-500R5Q systemd[1]: Starting The Apache HTTP Server...
12월 22 21:31:47 user1-500R5K-501R5K-500R5Q apachectl[9808]: AH00558: apache2: Could not reliably determine the >
12월 22 21:31:47 user1-500R5K-501R5K-500R5Q systemd[1]: Started The Apache HTTP Server.
```






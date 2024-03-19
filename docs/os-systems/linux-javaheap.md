---
layout: default
title: Java Heap 분석
parent: Linux
grand_parent: OS Systems
permalink: /docs/os-systems/linux-javaheap/
nav_order: 35
---

# Jps, Jmap,Jhat 을 이용한 Java Heap 분석
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

{: .note}
OOM 오류 시 분석 가능한 방법을 찾아본다. [https://imasoftwareengineer.tistory.com/4](https://imasoftwareengineer.tistory.com/4) 를 참조하여 작성하였다.



## 테스트 코드 작성 

최대한 많은 크기로 객체 크기 증가 시킨다.

## jps 를 통해서 process id 확인

`jps`


## jmap 를 통한 힙 덤프 생성

`jmap -dump:live,file=<dump-file> <pid>`

## jhat 을 통한 로컬서버에서의 힙덤프 분석

`jhat -J-Xmx<Memory_SIZE> -port <PORT> <HEAP_DUMP_PATH>`

## 참조자료 대로 분석






### groupadd

```
root@user1-500R5K-501R5K-500R5Q:~# groupadd testgroup1
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# grep testgroup1 /etc/group
testgroup1:x:1002:
```

### groupadd -g
`-g` 옵션을 사용하면 특정 GID 를 부여할 수 있다.

```
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# groupadd -g 1004 testgroup1004
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# grep testgroup1004 /etc/group
testgroup1004:x:1004:
```

### 이후의 groupadd

이후부터 생성되는 GID 는 가장 마지막 GID 다음 번호로 할당된다.

```
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# groupadd testgroup2
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# grep testgroup2 /etc/group
testgroup2:x:1005:
```

### /etc/group
각 그룹의 GID를 확인할 수 있다.

```
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# tail -f /etc/group
snapd-range-524288-root:x:524288:
snap_daemon:x:584788:
_cvsadmin:x:136:
jenkins:x:137:
docker:x:138:user1
postgres:x:139:
tg100:x:1001:
testgroup1:x:1002:
testgroup1004:x:1004:
testgroup2:x:1005:

```

### groupadd -r 

-r 옵션을 사용하면 0 번 부터 499 까지의 할당되어 있지 않은 GID 중 가장 높은 번호를 할당해 준다. 

```
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# groupadd -r testgroup3
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# grep testgroup3 /etc/group
testgroup3:x:998:
```

## 그룹 삭제

### groupdel

```
root@user1-500R5K-501R5K-500R5Q:~# groupdel testgroup3
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# grep testgroup3 /etc/group
root@user1-500R5K-501R5K-500R5Q:~# 
```

## 그룹 확인 

### groups

현재 사용자가 속한 그룹을 확인한다.

```
user1@user1-500R5K-501R5K-500R5Q:~$ groups
user1 adm cdrom sudo dip plugdev lpadmin lxd sambashare docker
```


## 그룹에 사용자 추가
 
특정 그룹에 사용자를 추가하거나 제거하고 특정 그룹에 패스워드를 설정하는 작업은 gpasswd를 이용한다. 

{: .important}
>
> -a user : 특정 그룹에 새로운 그룹멤버를 추가함.
>
> -d user : 특정 그룹에서 지정한 그룹멤버를 제거함.
>
> -r  : 특정 그룹의 패스워드를 제거함
>
> -R : 특정 그룹에 접근을 제한함
>
> -A user, ...  : 특정 그룹의 그룹관리자를 설정함
>
> -M user, ...  : 특정 그룹의 그룹멤버를 새로 설정함.
 

### gpasswd -a

user1 사용자를 testgroup2 에 추가

```
root@user1-500R5K-501R5K-500R5Q:~# gpasswd -a user1 testgroup2
Adding user user1 to group testgroup2
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# groups user1
user1 : user1 adm cdrom sudo dip plugdev lpadmin lxd sambashare docker testgroup2
```
### gpasswd -d

user1 사용자를 testgroup2 에서 삭제

```
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# gpasswd -d user1 testgroup2
Removing user user1 from group testgroup2
root@user1-500R5K-501R5K-500R5Q:~# 
root@user1-500R5K-501R5K-500R5Q:~# groups user1
user1 : user1 adm cdrom sudo dip plugdev lpadmin lxd sambashare docker
```




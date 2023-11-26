---
layout: default
title: Container 기반 기술
parent: Docker
grand_parent: Build & Package
permalink: /docs/buildpackage/docker-container-tech/
nav_order: 20
---

# Container 기반 기술
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

{: .highlight }
컨테이너 기반 기술 종류가 각각의 의미를 살펴보기로 한다.

## chroot

호스트 시스템과는 별도의 실행환경으로서, 루트 디렉토리 격리 기술 로 이해할 수 있으며 시스템 콜을 사용하여 프로세스가 바라보는 특정 디렉토리 루트(/) 디렉토리로 변경한다. 디렉토리를 격리하는 기술이다.

### chroot 프로그램 확인 및 신규 root 생성

```bash
root@docker1:~# pwd
/root
root@docker1:~# chroot --version
chroot (GNU coreutils) 8.28
Copyright (C) 2017 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

root@docker1:~# mkdir -p /root/newroot/{bin,lib,lib64}
root@docker1:~#
root@docker1:~# apt install tree
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  tree
0 upgraded, 1 newly installed, 0 to remove and 216 not upgraded.
Need to get 40.7 kB of archives.
After this operation, 105 kB of additional disk space will be used.
Get:1 http://kr.archive.ubuntu.com/ubuntu bionic/universe amd64 tree amd64 1.7.0-5 [40.7 kB]
Fetched 40.7 kB in 1s (30.6 kB/s)
Selecting previously unselected package tree.
(Reading database ... 169510 files and directories currently installed.)
Preparing to unpack .../tree_1.7.0-5_amd64.deb ...
Unpacking tree (1.7.0-5) ...
Setting up tree (1.7.0-5) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...

root@docker1:~# tree
.
├── labfilre.tgz
└── newroot
    ├── bin
    ├── lib
    └── lib64

4 directories, 1 file
```

루트의 bash 파일을 생성한 `newroot` 내에 복사한다.

```bash
root@docker1:~# ls -l /bin/bash
-rwxr-xr-x 1 root root 1113504 Jun  6  2019 /bin/bash
root@docker1:~# cp /bin/bash    /root/newroot/bin/bash
root@docker1:~# cp /bin/ls     /root/newroot/bin/ls
```

bash 가 참조하는 라이브러리를 새로 생성한 폴더에 복사한다.

> `ldd` : 프로그램이 참조하는 라이브러리를 확인할 수 있는 명령어

```bash
root@docker1:~# pwd
/root
root@docker1:~# ldd /bin/bash
 linux-vdso.so.1 (0x00007fffdd59d000)
 libtinfo.so.5 => /lib/x86_64-linux-gnu/libtinfo.so.5 (0x00007fbb244dd000)
 libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fbb242d9000)
 libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fbb23ee8000)
 /lib64/ld-linux-x86-64.so.2 (0x00007fbb24a21000)
root@docker1:~#
root@docker1:~# mkdir /root/newroot/lib/x86_64-linux-gnu
root@docker1:~#
root@docker1:~# cp /lib/x86_64-linux-gnu/libtinfo.so.5 /root/newroot/lib/x86_64-linux-gnu/libtinfo.so.5       root@docker1:~# cp /lib/x86_64-linux-gnu/libdl.so.2 /root/newroot/lib/x86_64-linux-gnu/libdl.so.2
root@docker1:~# cp /lib/x86_64-linux-gnu/libc.so.6 /root/newroot/lib/x86_64-linux-gnu/libc.so.6
root@docker1:~# cp /lib64/ld-linux-x86-64.so.2 /root/newroot/lib64/ld-linux-x86-64.so.2
root@docker1:~#
root@docker1:~#
root@docker1:~#
root@docker1:~# ldd /bin/ls
 linux-vdso.so.1 (0x00007ffcf237b000)
 libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007fb97f26f000)
 libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fb97ee7e000)
 libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007fb97ec0c000)
 libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fb97ea08000)
 /lib64/ld-linux-x86-64.so.2 (0x00007fb97f6b9000)
 libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fb97e7e9000)
root@docker1:~#
root@docker1:~# cp /lib/x86_64-linux-gnu/libselinux.so.1 /root/newroot/lib/x86_64-linux-gnu/libselinux.so.1
root@docker1:~# cp /lib/x86_64-linux-gnu/libc.so.6 /root/newroot/lib/x86_64-linux-gnu/libc.so.6
root@docker1:~# cp /lib/x86_64-linux-gnu/libpcre.so.3 /root/newroot/lib/x86_64-linux-gnu/libpcre.so.3
root@docker1:~# cp /lib/x86_64-linux-gnu/libdl.so.2 /root/newroot/lib/x86_64-linux-gnu/libdl.so.2
root@docker1:~# cp /lib64/ld-linux-x86-64.so.2 /root/newroot/lib64/ld-linux-x86-64.so.2
root@docker1:~# cp /lib/x86_64-linux-gnu/libpthread.so.0 /root/newroot/lib/x86_64-linux-gnu/libpthread.so.0
root@docker1:~#
root@docker1:~#
root@docker1:~# tree
.
├── labfilre.tgz
└── newroot
    ├── bin
    │   ├── bash
    │   └── ls
    ├── lib
    │   └── x86_64-linux-gnu
    │       ├── libc.so.6
    │       ├── libdl.so.2
    │       ├── libpcre.so.3
    │       ├── libpthread.so.0
    │       ├── libselinux.so.1
    │       └── libtinfo.so.5
    └── lib64
        └── ld-linux-x86-64.so.2

5 directories, 10 files
```

### `chroot` 적용 확인

`chroot` 명령어로 루트가 변경되는지 확인할 수 있다.

```bash
root@docker1:~# chroot /root/newroot/ /bin/bash
bash-4.4# pwd
/
bash-4.4#
bash-4.4# ls
bin  lib  lib64
bash-4.4#
bash-4.4# date
bash: date: command not found
bash-4.4#
bash-4.4# exit
exit
root@docker1:~#
```

## namespace

특정 프로세스(컨테이너)에 대해 시스템 리소스(운영체제 커널 관련 리소스) 를 논리적으로 격리하는 기능이며 아래로 구분할 수 있다.

-   `PID NameSpace` : 프로세스ID 를 분리하여 관리, 프로세스 목록을 따로 관리
-   `UTS NameSpace` : UNIX Time Sharing, hostname, NIS domain name 을 프로세스에게 독립적으로 할당한다.
-   `NET NameSpace` : 각 Network Namespace 별로 개별적인 인터페이스와 IP 구성을 가진다. Peering 형태로 연결한다

### PID NameSpace

init(`systemd`) 프로세스에게 부여도던 PID 1번을 각 PID 네임스페이스별로 할당한다. 또한 컨테이너별로 개별적인 PID 트리가 구성되기 때문에 PID 충돌없이 실행 가능하다.

> `ps -ef` : process status, -e every -f full

`-pid` 를 사용하여 unshare 한다.

```bash
root@docker1:~# unshare --pid --fork --mount-proc=/proc /bin/bash
root@docker1:~# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 06:00 pts/1    00:00:00 /bin/bash
root        11     1  0 06:00 pts/1    00:00:00 ps -ef
```

### UTS NameSpace

`uname()` 시스템 콜에 의해 반환되는 시스템 정보 중에서 hostname, NIS domain name 을 프로세스에게 독립적으로 할당한다. UTS 네임스페이스 별로 hostname 을 격리한다.

```bash
root@docker1:~# uname -n
docker1
root@docker1:~# hostname
docker1
root@docker1:~# ls -l /proc/$$/ns/uts
lrwxrwxrwx 1 root root 0 Jan  7 06:02 /proc/1/ns/uts -> 'uts:[4026531838]'
root@docker1:~#
root@docker1:~#
root@docker1:~# unshare -u /bin/bash
root@docker1:~# hostname happy.life.com
root@docker1:~#
root@docker1:~# hostname
happy.life.com
root@docker1:~#
root@docker1:~# uname -n
happy.life.com
root@docker1:~#
root@docker1:~# ls -l /proc/$$/ns/uts
lrwxrwxrwx 1 root root 0 Jan  7 06:04 /proc/16/ns/uts -> 'uts:[4026532254]'
root@docker1:~#
root@docker1:~# exit
exit
```

### NET NameSpace

각 Network Namespcace 별로 개별적인 인터페이스와 IP 구성을 가지며, 중복 포트 바인딩 등을 통한 네트워크 충돌을 방지한다.  
네트워크 장치, 주소, 경로 및 방화벽 규칙 같은 네트워크 자원을 격리한다.

```bash
root@docker1:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a7:a6:d4 brd ff:ff:ff:ff:ff:ff
    inet 192.168.137.101/24 brd 192.168.137.255 scope global enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fea7:a6d4/64 scope link
       valid_lft forever preferred_lft forever
root@docker1:~#
root@docker1:~# ip netns add guestnet  // guestnet 새로운 NET namespace 생성
root@docker1:~# ip netns ls
guestnet
root@docker1:~#
root@docker1:~# ip netns exec guestnet ip link // guestnet namespace 의 loopback 활성화
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
root@docker1:~# ip netns exec guestnet ip link set lo up
root@docker1:~# ip netns exec guestnet ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
root@docker1:~#
root@docker1:~#
root@docker1:~# ip link add host type veth peer name guest // default namespace 의 host(veth) 와 guestnet namespace guest(veth) 를 연결
root@docker1:~#
root@docker1:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:a7:a6:d4 brd ff:ff:ff:ff:ff:ff
3: guest@host: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether ce:cb:f9:46:8f:73 brd ff:ff:ff:ff:ff:ff
4: host@guest: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 02:09:d9:13:4f:e7 brd ff:ff:ff:ff:ff:ff
root@docker1:~#
root@docker1:~#
root@docker1:~# ip link set guest netns guestnet // guest(veth) 를 guestnet namespace 에 할당
root@docker1:~# ip link // guest@host 이 guestnet 으로 이동 확인
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:a7:a6:d4 brd ff:ff:ff:ff:ff:ff
4: host@if3: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether 02:09:d9:13:4f:e7 brd ff:ff:ff:ff:ff:ff link-netnsid 0
root@docker1:~#
root@docker1:~#
root@docker1:~# ip link set host up // host(veth) 에 IP 할당
root@docker1:~# ip address add 1.2.3.4/24 dev host
root@docker1:~#
root@docker1:~# ip netns exec guestnet ip link set guest up // guest(veth) 에 IP 할당
root@docker1:~# ip netns exec guestnet ip address add 1.2.3.5/24 dev guest
root@docker1:~# ip netns exec guestnet ip address show dev guest
3: guest@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether ce:cb:f9:46:8f:73 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 1.2.3.5/24 scope global guest
       valid_lft forever preferred_lft forever
    inet6 fe80::cccb:f9ff:fe46:8f73/64 scope link
       valid_lft forever preferred_lft forever
root@docker1:~#
root@docker1:~#
root@docker1:~# ping 1.2.3.5 // 통신 확인
PING 1.2.3.5 (1.2.3.5) 56(84) bytes of data. 
64 bytes from 1.2.3.5: icmp_seq=1 ttl=64 time=0.102 ms
64 bytes from 1.2.3.5: icmp_seq=2 ttl=64 time=0.068 ms
^C
--- 1.2.3.5 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1023ms
rtt min/avg/max/mdev = 0.068/0.085/0.102/0.017 ms
root@docker1:~#
root@docker1:~#
root@docker1:~# ip netns del guestnet // guestnet 삭제
root@docker1:~# ip netns ls
root@docker1:~#
```

## cGroups (Control Groups)

`cGroups` (`control Groups`) 는 커널이 가지고 있는 기능이며, `task` 라 불리는 프로세스 그룹에 대한 CPU, Memory, Network, Storage 자원 할당 및 제어하는 커널 기능이다.

### 사전 환경 구성

CPU 사용이 많은 환경을 구성하기 위해 아래의 프로그램을 작성한다.

> `gcc` : 처음에는 C 컴파일러였으며 `GNU C Compiler`의 약어였다. 하지만 기능이 추가되면서 `C++` 같은 다른 언어도 지원하게 되었고, `GNU Compiler Collectio`으로 이름을 변경하였다. 물론 약어는 여전히 GCC이다. 공식적으로 지원하는 언어는 `C`(gcc), `C++`(g++), `Objective-C`(gobjc), `Fortran`(gfortran), `Ada`(gnat), `Go`(gccgo), `D`(gdc)이다. `Java`(gcj)는 GCC 7.1 버전부터 지원이 중단되었다. GNU 진영에서는 GCC로 컴파일을 하고 Make를 이용해 빌드하는 것이 일반적이다.
> 
> `top` : 시스템의 상태를 전반적으로 가장 빠르게 파악 가능(CPU, Memory, Process) 하며 주로 CPU 사용률을 정렬해서 프로세스 목록을 출력하고자 할 떄 쓰인다.

```bash
# apt-get update 
# apt install -y gcc 

# nano a.c 
void main() {
        for(;;);
}

// 저장 시 ctrl+o (저장) > enter > ctrl+x (종료) 

# cat a.c 
# gcc a.c 
# cp a.out    b.out 
```

a.out 과 b.out 을 실행하면 아래와 같이 50% 의 동일한 CPU 를 점유한다.

```
# ./a.out &  // &: 백그라운드 실행
# ./b.out & 
```
 
![](/assets/images/dockercontainer1.png)


```bash

1024   :  1024   50%:50%
512    : 1024  (1:2    33%:66%)
256    : 1024  (1:4     20%:80%)


# cd /sys/fs/cgroup
# ls 
# cd cpu
# mkdir limit_50_percent
# cd limit_50_percent/
# ls 


# PS1="# "

#  echo $(pgrep a.out) > tasks             <-- 자원을 제어할 프로세스의 PID (a.out) 
+ echo 8385

# cat cpu.shares 

# echo 512 > cpu.shares <-- 경쟁이 발생했을 떄 상대적 가중치를 지정한다.(기본값은 1024)
# echo 256 > cpu.shares
```
 
![](/assets/images/dockercontainer2.png)

테스트 이후 실행한 프로세스를 종료한다.

```bash
# pkill b.out    (process kill : 프로세스 종료)
# pkill a.out 
```

## OverlayFS (in Union File System)

두개의 레이어만 가지고 있어 기존의 `AUFS` (`Advanced multi layered Unification File System`) 보다 단순한 디자이을 갖는다.  
이미지 레이어마다 `/var/lib/docker/overlay` 디렉토리 밑에 자신만의 디렉토리를 가진다.
 
![](/assets/images/dockercontainer3.png)

*[Overlay FS 구조]*
{: .text-center }



### 테스트 데이터 생성

아래와 같이 `container`, `image1`, `image2`, `work merge` 폴더 및 파일을 생성한다.

```bash
root@docker1:~# pwd
/root
root@docker1:~# mkdir overlayfs
root@docker1:~# mkdir container image1 image2 work merge
root@docker1:~# echo Hello, a > image1/a
root@docker1:~# echo Hello, b > image1/b
root@docker1:~# echo HEllo, c > image2/c
root@docker1:~# tree
.
├── a.c
├── a.out
├── b.out
├── container
├── image1
│   ├── a
│   └── b
├── image2
│   └── c
├── labfilre.tgz
├── merge
├── newroot
│   ├── bin
│   │   ├── bash
│   │   └── ls
│   ├── lib
│   │   └── x86_64-linux-gnu
│   │       ├── libc.so.6
│   │       ├── libdl.so.2
│   │       ├── libpcre.so.3
│   │       ├── libpthread.so.0
│   │       ├── libselinux.so.1
│   │       └── libtinfo.so.5
│   └── lib64
│       └── ld-linux-x86-64.so.2
├── overlayfs
├── tasks
└── work

11 directories, 17 files
root@docker1:~#
```

> `mount` : 기본 파일 시스템을 커널이 인식하도록 한다. 파일 시스템이 연결되면 마우트 지점이 마운트된 파일 시스템의 루트 디렉터리가 된다.

### 파일시스템 Overlay 마운트하기

`root@docker1:~# mount -t overlay overlay -o lowerdir=image2:image1,upperdir=container,workdir=work merge`

-   `overlay` : 타입 종류
-   `overlay` : 장치명
-   `lowerdir`, `upperdir`
-   `workdir` : 작업용 디렉토리
-   `merge` : 실제 참조되는 디렉토리

> `df` : 리눅스 시스템 전체의 (마운트 된) 디스크 사용량을 확인할 수 있습니다. 파일시스템, 디스크 크기, 사용량, 여유공간, 사용률, 마운트지점 순으로 나타납니다.  
> `-t` : type

```bash
root@docker1:~# mount -t overlay  overlay -o lowerdir=image2:image1,upperdir=container,workdir=work   merge
root@docker1:~#
root@docker1:~# df
Filesystem                        1K-blocks    Used Available Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv  19475088 6674836  11787928  37% /
udev                                1986084       0   1986084   0% /dev
tmpfs                               2019700       0   2019700   0% /dev/shm
tmpfs                                403940    1304    402636   1% /run
tmpfs                                  5120       4      5116   1% /run/lock
tmpfs                                403940      28    403912   1% /run/user/125
tmpfs                                403940       0    403940   0% /run/user/0
tmpfs                               2019700       0   2019700   0% /sys/fs/cgroup
/dev/sda2                            999320  153112    777396  17% /boot
overlay                            19475088 6674836  11787928  37% /root/merge
root@docker1:~#
```

`merge` 폴더에 overlay 된 파일들이 생성된다.

```bash
root@docker1:~# tree
.
├── a.c
├── a.out
├── b.out
├── container
├── image1
│   ├── a
│   └── b
├── image2
│   └── c
├── labfilre.tgz
├── merge
│   ├── a
│   ├── b
│   └── c
├── newroot
│   ├── bin
│   │   ├── bash
│   │   └── ls
│   ├── lib
│   │   └── x86_64-linux-gnu
│   │       ├── libc.so.6
│   │       ├── libdl.so.2
│   │       ├── libpcre.so.3
│   │       ├── libpthread.so.0
│   │       ├── libselinux.so.1
│   │       └── libtinfo.so.5
│   └── lib64
│       └── ld-linux-x86-64.so.2
├── overlayfs
├── tasks
└── work
    └── work

12 directories, 20 files
```

`merge` 파일 내용을 변경하면 `container` 에 반영이 된다.

```bash
root@docker1:~# cat merge/a
Hello, a
root@docker1:~# echo Hello, yumi > merge/a
root@docker1:~# cat merge/a
Hello, yumi
root@docker1:~# cat container/a
Hello, yumi
root@docker1:~#
```

`merge` 에 파일을 생성하면 `container` 에도 생성이 된다.

```bash
root@docker1:~#
root@docker1:~# echo Hello, d > merge/d
root@docker1:~# tree
.
├── a.c
├── a.out
├── b.out
├── container
│   ├── a
│   └── d
├── image1
│   ├── a
│   └── b
├── image2
│   └── c
├── labfilre.tgz
├── merge
│   ├── a
│   ├── b
│   ├── c
│   └── d
├── newroot
│   ├── bin
│   │   ├── bash
│   │   └── ls
│   ├── lib
│   │   └── x86_64-linux-gnu
│   │       ├── libc.so.6
│   │       ├── libdl.so.2
│   │       ├── libpcre.so.3
│   │       ├── libpthread.so.0
│   │       ├── libselinux.so.1
│   │       └── libtinfo.so.5
│   └── lib64
│       └── ld-linux-x86-64.so.2
├── overlayfs
├── tasks
└── work
    └── work

12 directories, 23 files
```


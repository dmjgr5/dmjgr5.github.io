---
layout: default
title: Linux Command
parent: Linux
grand_parent: OS Systems
permalink: /docs/os-systems/linux-command/
nav_order: 20
---

# Linux Command
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

## rename -a, hostnamectl 

리눅스 커널 정보를 출력한다.


```sh 
~$ uname -a
Linux dcpark-500R5K-501R5K-500R5Q 5.15.0-92-generic #102~20.04.1-Ubuntu SMP Mon Jan 15 13:09:14 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
~$ hostnamectl
   Static hostname: test-500R5K-501R5K-500R5Q
         Icon name: computer-laptop
           Chassis: laptop
        Machine ID: 60c7a7e6eec344e095e9391aa8131f7d
           Boot ID: f54e9139e9f449bda63e721c9777a514
  Operating System: Ubuntu 20.04.6 LTS
            Kernel: Linux 5.15.0-92-generic
      Architecture: x86-64
```

## lsb_release -a

배포 버전을 확인할 수 있다. 

```sh
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.6 LTS
Release:	20.04
Codename:	focal
```
 

## df -h

Disk 용량을 확인할 수 있다.

```bash
df : 디스크 남은 용량 확인 (기본 명령어)
df -k : 킬로바이트 단위로 남은 용량 확인
df -m : 메가 바이트 단위로 남은 용량 확인
df -h : 깔끔하게 보여줌
df . : 현재 디렉토리가 포함된 파티션의 남은 용량 확인
```

```sh
~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            3.8G     0  3.8G   0% /dev
tmperfs           785M  2.2M  783M   1% /run
/dev/sda2       117G   37G   75G  33% /
tmpfs           3.9G  143M  3.7G   4% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
/dev/loop0      160M  160M     0 100% /snap/chromium/2738
/dev/loop3       56M   56M     0 100% /snap/core18/2796
```

## du -sh

선택한 폴더나 파일의 용량을 확인한다.

```
du : 현재 디렉토리에서 서브 디렉토리까지 용량 확인
du -a : 현재 디렉토리의 사용량 파일단위로 출력
du -s : 총 사용량
du -h : 깔끔하게 보여줌
du -sh * : 한단계 서브 디렉토리 기준으로 보여줌
```

```sh
$ du -sh Templates/
4.0K	Templates/
```



## lscpu

```bash
~$ lscpu
Architecture:                       x86_64
CPU op-mode(s):                     32-bit, 64-bit
Byte Order:                         Little Endian
Address sizes:                      39 bits physical, 48 bits virtual
CPU(s):                             4
On-line CPU(s) list:                0-3
Thread(s) per core:                 2
Core(s) per socket:                 2
```

{: .important}
> CPU 개수만 조회할 경우 `grep -c processor /proc/cpuinfo` 명령어를 활용할 수 있다.


## free -h      

메모리를 확인한다. -h 옵션은 사람이 읽기 쉬운 단위로 출력한다.


```bash
$ free -h
              total        used        free      shared  buff/cache   available
Mem:          7.7Gi       4.6Gi       216Mi       546Mi       2.8Gi       2.2Gi
Swap:            0B          0B          0B

```


## systemctl list-units -t service

service 프로세스 항목과 상태를 조회한다.

```sh
user:~$ systemctl list-units -t service 
  UNIT                                                  LOAD   ACTIVE SUB     DESCRIPTION                                                       
  accounts-daemon.service                               loaded active running Accounts Service                                                  
  acpid.service                                         loaded active running ACPI event daemon                                                 
  alsa-restore.service                                  loaded active exited  Save/Restore Sound Card State   
```


## ps -ef

전체적인 프로세스와 관련된 옵션이다.

-   `-e` : 커널 프로세스를 제외한 모든 프로세스를 보여준다.
-   `f` : UID, PID, PPID, C, STIME, TTY, TIME, CMD 등의 필드목록으로, CMD필드의 전체 명령어 형태를 보여준다.

```bash
root@master:~/labfile/service# ps -ef | grep docker
root      1485     1  1 Jan14 ?        00:11:50 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
root      1939  6201  0 02:39 ?        00:00:00 runc --root /var/run/docker/runtime-runc/moby --log /run/containerd/io.containerd.runtime.v2.task/moby/f15696723766975a8c120d6cd8a6d77161135b013c13ed80c158331843cd1a6a/log.json --log-format json --systemd-cgroup exec --process /tmp/runc-process236202202 --detach --pid-file /run/containerd/io.containerd.runtime.v2.task/moby/f15696723766975a8c120d6cd8a6d77161135b013c13ed80c158331843cd1a6a/0a2cb1a2031224ed66bb0bb8b9a23bc295c7b0c7d29a37ed774f14d8072f8f6d.pid f15696723766975a8c120d6cd8a6d77161135b013c13ed80c158331843cd1a6a
root      1957 15646  0 02:39 pts/0    00:00:00 grep --color=auto docker
root@master:~/labfile/service#
```

## curl

`curl`(client url) 명령어는 프로토콜들을 이용해 URL 로 데이터를 전송하여 서버에 데이터를 보내거나 가져올때 사용하기 위한 명령줄 도구 및 라이브러리이다.

쉽게말해 예를들어 자바스크립트 환경에서 REST API(http)를 테스트하고싶다면 보통 ajax, fetch 를 이용해 요청을 보내는 것과 같이, SHELL(커맨드라인 환경)에서 REST API(http) 테스트 하고 싶으면 curl 명령어를 이용하면 된다 라고 이해하면 된다.



`-s` : --silent 진행 내용이나 메시지들을 출력하지 않음  
`-f` : --fail HTTP 오류 시 자동으로 실패 (출력 없음)

```
root@master:~/labfile/service# curl http://10.105.74.121 | grep Hello
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   318  100   318    0     0  63600      0 --:--:-- --:--:-- --:--:-- 63600
        <p>Hello,  hostname-server-7c85cc96dc-2j9lv</p> </blockquote>
root@master:~/labfile/service#
root@master:~/labfile/service#
root@master:~/labfile/service# curl -sf http://10.105.74.121 | grep Hello
        <p>Hello,  hostname-server-7c85cc96dc-2j9lv</p> </blockquote>
root@master:~/labfile/service#
```

## sh 또는 ./

`.sh` 확장자가 나오길래, 뭐지 싶었는데 찾아보니 shell script 확장자였다.

실행하는 방법은 파일 권한을 변경 후 실행하면 된다...고 한다.

우선 파일 권한은 chmod 명령어를 사용하여 변경해준다.  
744 등을 주어도 되지만, 간단하게  
`$chmod +x filename.sh` <-- 쓰기 권한 추가하기 로 실행이 된다.

> `sh filename.sh` 또는 `./filename.sh` 으로 실행한다.

```bash
root@master:~# ./get_helm.sh
Downloading https://get.helm.sh/helm-v3.10.3-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
root@master:~#
```

## tar

`tar -zcvf target.tar.gz file1 file2 dir1 dir2` : 파일 압축  
`tar -zxvf target.tar.gz` : 압축 해제

> `-c` : --create 신규 아카이브 생성  
> `-x` : --extract 아카이브 풀기  
> `-v` : --verbose 처리 과정을 상세히 표시  
> `-f` : --file=ARCHIVE 파일을 대상으로 아카이빙

```bash
root@master:~# tar -zcvf testtar2.tar.gz a.out b.out
a.out
b.out
root@master:~#
root@master:~#
root@master:~# mv testtar1.tar.gz ./test/testtar1.tar.gz^C
root@master:~# mkdir testtar
root@master:~# mv testtar1.tar.gz ./testtar/testtar1.tar.gz
root@master:~#
root@master:~# cd testtar/
root@master:~/testtar# ls
testtar1.tar.gz
root@master:~/testtar# tar zxvf testtar1.tar.gz
a.out
b.out
root@master:~/testtar# ls
a.out  b.out  testtar1.tar.gz
```

```bash
root@docker1:~/labfile/dockerfile_dir# ls
add_data.tgz   Dockerfile    Dockerfile.11  Dockerfile.3  Dockerfile.5  host.html
copy_data.tgz  Dockerfile.1  Dockerfile.2   Dockerfile.4  Dockerfile.6  index.html
root@docker1:~/labfile/dockerfile_dir#

// 파일 압축
root@docker1:~/labfile/dockerfile_dir# tar -cvf target.tar Dockerfile.1 Dockerfile.2
Dockerfile.1
Dockerfile.2
root@docker1:~/labfile/dockerfile_dir# ls
add_data.tgz   Dockerfile.1   Dockerfile.3  Dockerfile.6  target.tar
copy_data.tgz  Dockerfile.11  Dockerfile.4  host.html
Dockerfile     Dockerfile.2   Dockerfile.5  index.html
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir#  

// 압축 해제
root@docker1:~/labfile/dockerfile_dir# tar -xvf target.tar
Dockerfile.1
Dockerfile.2
root@docker1:~/labfile/dockerfile_dir#
root@docker1:~/labfile/dockerfile_dir# ls
add_data.tgz   Dockerfile.1   Dockerfile.3  Dockerfile.6  target.tar
copy_data.tgz  Dockerfile.11  Dockerfile.4  host.html
Dockerfile     Dockerfile.2   Dockerfile.5  index.html
root@docker1:~/labfile/dockerfile_dir#

```


## Listen port 확인하기

아래와 같이 3가지 방법으로 확인이 가능하다.


### sudo netstat -ltup

`Netstat` 은 아래와 같이 open 되어있는 모든 listen port를 확인하는데 사용할 수 있다.

 

- -l 옵션 : netstat에 모든 수신 소켓을 표시한다.
- -t 옵션 : 모든 TCP 연결을 표시한다.
- -u 옵션 : 모든 UDP 연결을 표시한다.
- -p 옵션 : 포트에서 수신하는 어플리케이션/데몬의 이름을 표시한다.
- -n 옵션 : 서비스 이름 대신 port 번호를 표시한다.

```bash
user@user:~$ sudo netstat -ltup
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 localhost:ipp           0.0.0.0:*               LISTEN      899/cupsd           
tcp        0      0 localhost:37485         0.0.0.0:*               LISTEN      1034/containerd     
tcp        0      0 localhost:postgresql    0.0.0.0:*               LISTEN      1129/postgres       
tcp        0      0 localhost:domain        0.0.0.0:*               LISTEN      796/systemd-resolve 
tcp        0      0 localhost:4000          0.0.0.0:*               LISTEN      35497/ruby2.7       
tcp6       0      0 ip6-localhost:ipp       [::]:*                  LISTEN      899/cupsd           
tcp6       0      0 [::]:9090               [::]:*                  LISTEN      982/java            
udp        0      0 0.0.0.0:58210           0.0.0.0:*                           894/avahi-daemon: r 
udp        0      0 mdns.mcast.net:mdns     0.0.0.0:*                           2999/chrome         
udp        0      0 0.0.0.0:mdns            0.0.0.0:*                           894/avahi-daemon: r 
udp        0      0 testuser-500R5K-501:34197 0.0.0.0:*                           2999/chrome         
udp        0      0 localhost:domain        0.0.0.0:*                           796/systemd-resolve 
udp        0      0 0.0.0.0:631             0.0.0.0:*                           975/cups-browsed    
udp6       0      0 [::]:mdns               [::]:*                              894/avahi-daemon: r 
udp6       0      0 [::]:44146              [::]:*                              894/avahi-daemon: r 
```

### sudo ss -lntu

netstat 과 유사하여, 위 옵션은 TCP/UDP 에대한 수신 대기 포트를 숫자로 표시한다.

```sh 
~$ sudo ss -lntu
Netid            State             Recv-Q             Send-Q                         Local Address:Port                          Peer Address:Port            Process            
udp              UNCONN            0                  0                                    0.0.0.0:57192                              0.0.0.0:*                                  
udp              UNCONN            0                  0                              127.0.0.53%lo:53                                 0.0.0.0:*                                  
udp              UNCONN            0                  0                                    0.0.0.0:631                                0.0.0.0:*                                  
udp              UNCONN            0                  0                                224.0.0.251:5353                               0.0.0.0:*                                  
udp              UNCONN            0                  0                                224.0.0.251:5353                               0.0.0.0:*                                  
udp              UNCONN            0                  0                                    0.0.0.0:5353                               0.0.0.0:*                                  
udp              UNCONN            0                  0                                       [::]:38371                                 [::]:*                                  
udp              UNCONN            0                  0                                          *:1716                                     *:*                                  
udp              UNCONN            0                  0                                       [::]:5353                                  [::]:*                                  
tcp              LISTEN            0                  4096                               127.0.0.1:41397                              0.0.0.0:*                                  
tcp              LISTEN            0                  4096                           127.0.0.53%lo:53                                 0.0.0.0:*                                  
tcp              LISTEN            0                  511                                  0.0.0.0:8887                               0.0.0.0:*                                  
tcp              LISTEN            0                  5                                  127.0.0.1:631                                0.0.0.0:*                                  
tcp              LISTEN            0                  244                                127.0.0.1:5432                               0.0.0.0:*                                  
tcp              LISTEN            0                  4096                               127.0.0.1:4000                               0.0.0.0:*                                  
tcp              LISTEN            0                  50                                         *:1716                                     *:*                                  
tcp              LISTEN            0                  5                                      [::1]:631                                   [::]:*                                  
tcp              LISTEN            0                  511                                        *:15611                                    *:*                                  
tcp              LISTEN            0                  50                                         *:9090                                     *:*         
```

### sudo lsof -i:포트번호

openfile 에서 확인하며 -i 옵션은 모든 네트워크 파일을 확인할 수 있다. 특정 포트를 확인하고 싶은 경우는 :포트번호를 입력한다.

```sh
~$ sudo lsof -i:4000
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
ruby2.7 7795 root   11u  IPv4  87943      0t0  TCP localhost:4000 (LISTEN)
```



## alias

`# nano /root/.bashrc` 파일에서 맨 아래 줄에 다음 내용 추가한다.

```bash
alias conrm='docker container rm -f $(docker container ps -aq)'
```

이후 ctrl+o (저장) > enter > ctrl+x (종료) 를 통해 저장한다.

## rsync

rsync 는 Remote Sync 의 약자로, rsync 를 통해 서버 간의 동기화나 백업을 진행할 수 있다.


사용 방법은 아래와 같다.

`rsync options src dest`



{: .note}
> -a, --archive : 압축 모드
>
> -v, --verbose : 상세한 정보 출력
> 
> -h : human-readable, output numbers in a human-readable format
 
## diff 

diff 는 두 파일 사이의 내용을 비교하는 명령어이다.

{: .note}
> -r : 두 디렉토리간의 차이점 출력, 서브디렉토리 까지 비교


rsync 와 diff 에 대한 예제를 아래와 같이 확인해 보자.


```sh 
# 파일 비교
user1@user1-500R5K-501R5K-500R5Q:~$ ls ./Documents/
mystudy.md  _.naver.com

# rsync
user1@user1-500R5K-501R5K-500R5Q:~$ rsync -avh ./Documents/ ./backup
sending incremental file list
created directory ./backup
./
_.naver.com
mystudy.md

sent 8.07K bytes  received 88 bytes  16.31K bytes/sec
total size is 7.87K  speedup is 0.96
user1@user1-500R5K-501R5K-500R5Q:~$ 


# rsync 결과 확인 - 폴더와 함꼐 파일이 생성됨
user1@user1-500R5K-501R5K-500R5Q:~$ ls ./backup/
mystudy.md  _.naver.com
user1@user1-500R5K-501R5K-500R5Q:~$ 

# 파일 비교 - 내용이 없으므로 동일함 확인
user1@user1-500R5K-501R5K-500R5Q:~$ diff -r ./Documents/ ./backup/


# 파일 수정
user1@user1-500R5K-501R5K-500R5Q:~$ vi ./Documents/mystudy.md 


# 파일 비교 - 내용이 다름을 확인
user1@user1-500R5K-501R5K-500R5Q:~$ diff -r ./Documents/ ./backup/
diff -r ./Documents/mystudy.md ./backup/mystudy.md
2,9d1
< remote-theme: just-the-docs/just-the-docs
< update
< 1
< 2
< 3
< 
< 
< remote-theme: just-the-docs/just-the-docs
193c185
< [Spring Boot + Nginx 연동해서 배포하기](https://velog.io/@u-nij/Spring-Boot-Nginx-%EC%97%B0%EB%8F%99%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)
---
> [Spring Boot + Nginx 연동해서 배포하기](https://velog.io/@u-nij/Spring-Boot-Nginx-%EC%97%B0%EB%8F%99%ED%95%B4%EC%84%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)
\ No newline at end of file

# rsync - 다른 부분을 rsync
user1@user1-500R5K-501R5K-500R5Q:~$ rsync -avh ./Documents/ ./backup
sending incremental file list
./
mystudy.md

sent 5.48K bytes  received 38 bytes  11.03K bytes/sec
total size is 7.97K  speedup is 1.44

# 파일 비교 - 내용 동일
user1@user1-500R5K-501R5K-500R5Q:~$ diff -r ./Documents/ ./backup/
user1@user1-500R5K-501R5K-500R5Q:~$ 
user1@user1-500R5K-501R5K-500R5Q:~$ 

```
## vi 편집기

### yy , #yy	
커서가 위치한 행을 복사한다. # 에는 복사할 행의 수를 지정


### p	

커서가 위치한 행의 아래쪽에 붙인다.

### P	

커서가 위치한 행의 위쪽에 붙인다.

### dd, #dd	

커서가 위치한 행을 잘라둔다. 삭제와 같은 기능 , # 에는 잘라둘 행의 수를 지정한다. 

### /{키워드}

찾기 명령어로, 다음 찾기는 n 이전 찾기는 N 으로 찾을 수 있다.



### ctrl+f , ctrl+b

다음 또는 이전 페이지로 이동한다.

### gg, G

파일의 제일 첫 문장이나 끝 문장으로 이동한다.
 
 
### :set number, :set nonumber

좌측에 라인번호를 표시하거나 숨길 수 있다.


### :{숫자}  

숫자 번째 문장으로 이동한다.
 

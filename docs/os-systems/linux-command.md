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


## Process 확인하기


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

## Curl 사용하기

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

## Shell 스크립트 실행하기

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

## 파일 압축 및 해제하기

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

## CPU 정보 확인하기

```bash
root@docker1:~# lscpu
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              1
On-line CPU(s) list: 0
Thread(s) per core:  1
Core(s) per socket:  1
Socket(s):           1
NUMA node(s):        1
Vendor ID:           GenuineIntel
CPU family:          6
Model:               140
Model name:          11th Gen Intel(R) Core(TM) i7-1165G7 @ 2.80GHz
Stepping:            1
CPU MHz:             2803.200
BogoMIPS:            5606.40
Hypervisor vendor:   KVM
Virtualization type: full
L1d cache:           48K
L1i cache:           32K
L2 cache:            1280K
L3 cache:            12288K
NUMA node0 CPU(s):   0
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq monitor ssse3 cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single fsgsbase avx2 invpcid rdseed clflushopt md_clear flush_l1d arch_capabilities
root@docker1:~#
```

## Disk 용량 확인 하기

```bash
df : 디스크 남은 용량 확인 (기본 명령어)
df -k : 킬로바이트 단위로 남은 용량 확인
df -m : 메가 바이트 단위로 남은 용량 확인
df -h : 깔끔하게 보여줌
df . : 현재 디렉토리가 포함된 파티션의 남은 용량 확인
du : 현재 디렉토리에서 서브 디렉토리까지 용량 확인
du -a : 현재 디렉토리의 사용량 파일단위로 출력
du -s : 총 사용량
du -h : 깔끔하게 보여줌
du -sh * : 한단계 서브 디렉토리 기준으로 보여줌
```

## RAM 사이즈 확인하기

```bash
root@docker1:~# free
              total        used        free      shared  buff/cache   available
Mem:        4039328     1140088     1636088       34088     1263152     2608324
Swap:       2017276           0     2017276
root@docker1:~#
```

## Listen Port 확인하기

`Netstat` 은 아래와 같이 open 되어있는 모든 port를 확인하는데 사용할 수 있다.

 

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




## Alias 설정하기

`# nano /root/.bashrc` 파일에서 맨 아래 줄에 다음 내용 추가한다.

```bash
alias conrm='docker container rm -f $(docker container ps -aq)'
```

이후 ctrl+o (저장) > enter > ctrl+x (종료) 를 통해 저장한다.

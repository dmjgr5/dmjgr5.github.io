---
layout: default
title: Docker Volume
parent: Docker
grand_parent: Cloud
permalink: /docs/cloud/docker-volume/
nav_order: 24
---

# Docker Volume
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
컨테이너 내에서 생성 또는 변경된 모든 파일은 `Read-Write` 가능한 Container Layer 에 저장된다. 도커는 Copy-on-write (`CoW`) 방식으로 파일을 관리한다.  
컨테이너가 삭제되면, 데이터가 영구적으로 유지되지 않기 때문에 다른 프로세스나 컨테이너가 해당 데이터를 필요로 하는 경우 사용할 수 없게 된다. 이 때문에 `Volume` 즉, 영구적인 저장소가 필요하다.

 

## 컨테이너의 기본 UpperDir 위치 확인하기

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

## 도커 마운트 타입
 
  
![](/assets/images/dockervolume1.png)

*[Docker Mount Type]*
{: .text-center }
  

### `bind mount`

도커 초기에 사용하던 방식이며, 호스트 싯스템의 전체 경로를 통해 참조됨에 따라 호스트 시스템의 File System 디렉터리 구조에 의존적이다.

```bash
root@docker1:~# mkdir /tmp/hostdir
root@docker1:~# mkdir /tmp/readonly
root@docker1:~# echo writable > /tmp/hostdir/file1
root@docker1:~# echo readonly > /tmp/readonly/file2

// container_dir 는 기본, read_dir 는 read only 로 설정한다.
root@docker1:~# docker container run -it -v /tmp/hostdir:/container_dir -v /tmp/readonly:/read_dir:ro --name test01 centos

// 컨테이너에 바인딩 확인
[root@9dc954f8a1fa /]# cat /container_dir/file1
writable
[root@9dc954f8a1fa /]# cat /read_dir/file2
readonly

// 컨테이너 내 container_dir 폴더에 쓰기 가능
[root@9dc954f8a1fa /]# echo Hello, file1 > /container_dir/file1
[root@9dc954f8a1fa /]# cat /container_dir/file1
Hello, file1

// 컨테이너 내 read_dir 폴더에 쓰기 불가
[root@9dc954f8a1fa /]# echo Hello, file2 > /read_dir/file2
bash: /read_dir/file2: Read-only file system
[root@9dc954f8a1fa /]# echo Hello, file3 > /read_dir/file3
bash: /read_dir/file3: Read-only file system
[root@9dc954f8a1fa /]# echo Hello, file3 > /container_dir/file3
[root@9dc954f8a1fa /]# echo Hello, file4 > /file4
[root@9dc954f8a1fa /]# exit
exit
root@docker1:~# docker container rm test01
test01

// 컨테이너에서 작성한 file3 바인딩 확인
root@docker1:~# ls /tmp/hostdir
file1  file3
root@docker1:~# ls /tmp/readonly
file2
```

컨테이너의 바인딩 정보는 `docker container inspect` 에서 `Mounts` 키워드로 확인할 수 있다.

```bash
root@docker1:~# docker container run -it -v /tmp/hostdir:/container_dir -v /tmp/readonly:/read_dir:ro --name test01 centos
[root@6c8284413747 /]# root@docker1:~#
root@docker1:~# docker container inspect test01 | grep -A 20 Mounts
        "Mounts": [
            {
                "Type": "bind",
                "Source": "/tmp/hostdir",
                "Destination": "/container_dir",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            },
            {
                "Type": "bind",
                "Source": "/tmp/readonly",
                "Destination": "/read_dir",
                "Mode": "ro", // read-only
                "RW": false,
                "Propagation": "rprivate"
            }
        ],
        "Config": {
            "Hostname": "6c8284413747",
            "Domainname": "",
```

### `volume`

`docker voulme create` 명령을 통해서 미리 생성되거나 컨테이너가 생성될 때 생성된 볼륨을 컨테이너에 마운트해서 사용가능하다.  
볼륨의 기본 경로는 `/var/lib/docker/volumns/` 에 위치하며 컨테이너가 삭제/정지 되더라도 지워지지 않는다. 삭제시에는 `docker volume rm` 을 통해 삭제 가능하다.  
`volume` 마운트 타입 구성 방법에는 아래와 같이 3가지가 있다.

#### 볼륨 자동 생성

`-v` 옵션으로 컨테이너 생성 시 자동 생성되면 이름이 길어 식별하기 어렵다.

```bash
root@docker1:~# docker container run -it -v /volume --name test02 centos

 // 컨테이너 /volume 에 passwd 파일 붙여넣기
[root@3d88f4ab4eaa /]# ls /volume/
[root@3d88f4ab4eaa /]# cp /etc/passwd /volume
[root@3d88f4ab4eaa /]# ls /volume/
passwd
[root@3d88f4ab4eaa /]#

// detach 모드로 전환
[root@3d88f4ab4eaa /]# root@docker1:~#

// volume 정보 조회
root@docker1:~# docker volume ls
DRIVER    VOLUME NAME
local     fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d
root@docker1:~#

// volume 상세 정보 조회
root@docker1:~# docker volume inspect fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d
[
    {
        "CreatedAt": "2023-01-08T13:39:29Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d/_data",
        "Name": "fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d",
        "Options": null,
        "Scope": "local"
    }
]

// 호스트에서의 volume 데이터 확인
root@docker1:~# ls /var/lib/docker/volumes/fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d/_data
passwd

// 컨테이너 마운트 정보 조회
root@docker1:~# docker container inspect test02 | grep -A 10 Mounts
        "Mounts": [
            {
                "Type": "volume",
                "Name": "fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d",
                "Source": "/var/lib/docker/volumes/fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d/_data",
                "Destination": "/volume",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
```

#### `named volume` 생성

여러 컨테이너 간에 볼륨을 보다 안전하게 공유할 수 있으며 암호화 가능하다. 컨텐츠를 사전에 채워 놓을 수 있다.

```bash
root@docker1:~# docker volume ls
DRIVER    VOLUME NAME
local     fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d
root@docker1:~#

// persistvol 볼륨 생성
volume  create persistvol
persistvol
root@docker1:~# docker volume ls
DRIVER    VOLUME NAME
local     fba67fc43867c48a032bd2f778857feeb48cf3f79e8d2b1ff15b83edda28853d
local     persistvol

// persistvol 볼륨 상세 조회
root@docker1:~# docker volume inspect persistvol
[
    {
        "CreatedAt": "2023-01-08T13:52:07Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/persistvol/_data",
        "Name": "persistvol",
        "Options": {},
        "Scope": "local"
    }
]

// persistvol 볼륨을 참조하는 vol_container 컨테이너 생성
root@docker1:~# docker container run -it --name vol_container -v persistvol:/container  centos
[root@7ccd49ed19a5 /]# ls /container
[root@7ccd49ed19a5 /]# echo Hello, a > /container/a
[root@7ccd49ed19a5 /]# ls /container
a
[root@7ccd49ed19a5 /]# root@docker1:~#
root@docker1:~#
```

#### 볼륨 컨테이너 사용하기

`-v` 옵션으로 볼륨을 사용하는 컨테이너를 다른 컨테이너와 공유할 수 있다. 컨테이너 생성 시 `--volumes-from` 옵션을 설정하면 `-v` 옵션을 적용한 컨테이너의 볼륨 디렉토리를 공유할 수 있다. 또한 볼륨 컨테이너가 꺼져 있어도 사용할 수 있다.

```bash
// vol_container 컨테이너의 볼륨을 참조하는 data_container 컨테이너 생성 
root@docker1:~# docker container run -it --volumes-from vol_container --name data_container centos

// 동일 데이터 확인
[root@41f2f04a995d /]# cat /container/a
Hello, a
[root@41f2f04a995d /]# echo Hello, yumi > /container/a
[root@41f2f04a995d /]# cat /container/a
Hello, yumi
[root@41f2f04a995d /]# root@docker1:~#

// data_container 가 참조하는 볼륨 상세 조회
root@docker1:~# docker container inspect data_container | grep -A 10 Mounts
        "Mounts": [
            {
                "Type": "volume",
                "Name": "persistvol",
                "Source": "/var/lib/docker/volumes/persistvol/_data",
                "Destination": "/container",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
root@docker1:~#
```

### `tmpfs`

호스트의 메모리 공간에 저장되기 때문에 컨테이너 정지 시 삭제되나, 빠른 I/O 목적으로 할 경우에는 장점이 된다.
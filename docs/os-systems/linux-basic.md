---
layout: default
title: Linux Basics
parent: Linux
grand_parent: OS Systems
permalink: /docs/os-systems/linux-basics/
nav_order: 10
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


주요 개념을 정리해 본다.

---

## PID = 1 번 프로세스는 무엇인가

PID = 1 번 프로세스는 `systemd` 이다.

```bash
root@docker1:~# top
top - 15:55:44 up 57 min,  2 users,  load average: 0.00, 0.08, 0.07
Tasks: 208 total,   1 running, 165 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.7 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  4039384 total,  1208128 free,  1027444 used,  1803812 buff/cache
KiB Swap:  2017276 total,  2017276 free,        0 used.  2730296 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1896 root      20   0  107788   7120   6116 S  0.3  0.2   0:00.59 sshd
 4038 root      20   0   42952   3992   3300 R  0.3  0.1   0:00.04 top
    1 root      20   0  160148   9288   6548 S  0.0  0.2   0:02.30 systemd
```

---

## 파일 권한 알아보기

### 파일 종류 및 유저/그룹

파일 권한은 4개로 등분하여 확인할 수 있다.


![](/assets/images/linux1.png)




-   1번은 파일의 종류
-   2번은 파일과 폴더의 유저 권한
-   3번은 파일과 폴더의 그룹 권한
-   4번은 파일에 대한 모든 유저의 권한

### 약자의 의미

-   r : read
-   w : wrtie
-   x : excute
-   d : Directory
-   r : Regular file
-   s : Symbolic link
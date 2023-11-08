---
layout: default
title: 가상머신 이미지 가져오기
parent: Docker
grand_parent: Cloud
permalink: /docs/cloud/docker-install-image-vb/
nav_order: 15
---

# Virtual Box 에서 가상머신 이미지 가져오기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
본 장은 `master.ova` 이미지 파일에 docker 가 설치되어 있다는 가정하에 docker 실습을 위한 환경 설정을 구성한다.

## 실습 전 확인 사항

명령 프롬프트에서 `systeminfo` 명령어를 통해 Hyper-V 요구 사항이 `예` 로 되어 있는지 확인한다.

```bash
C:\Users\dcjam>systeminfo

호스트 이름:             DESKTOP-605KI17

... 중간 생략 ...

Hyper-V 요구 사항:       VM 모니터 모드 확장: 예
                         펌웨어에 가상화 사용: 예
                         두 번째 수준 주소 변환: 예
                         데이터 실행 방지 사용 가능: 예
```

## VirtualBox 프로그램 설치

### VirtualBox 설치

[www.virtualbox.org](http://www.virtualbox.org) 에서 다운로드 가능하며, VirtualBox-6.1.32-149290-Win.exe 를 실행한다.

### 가상머신 이미지 가져오기

파일 > 가상시스템 가져오기 > `master.ova` 파일 선택 > 다음 클릭 후 아래를 선택한다.

-   USB 컨트롤러 : 선택 해제
-   MAC 주소 정책 : 모든 네트워크 어댑터의 새 MAC 주소 생성  
    이후 가져오기 를 클릭한다.

가져오기 시 오류 발생하면 C:\\Users\\HPE\\VirtualBox VMs\\master 폴더 삭제 후 다시 진행한다.

### 가상 머신 시작하기

Oracle VM VirtualBox 관리자에서 `master` 가상 머신 선택하여 시작을 클릭한다.  
이후 로그인 창에서 worker1 선택 `ubuntu` 를 패스워드로 하여 로그인 진행한다.

-   화면 해상도 변경하기
    -   우측 상단 (전원) --> 드라이버 & 스패너(설정)
    -   Power > Blank screen > Never
    -   Devices -> Resolution > 1600 x 1200 > Apply Changes? (더블클릭) > Apply > Keep Changes
        -   바탕화면에 마우스 올려놓으시고 "우클릭" --> Open Terminal

### 사용자 전환 및 네트워크 연결 체크

-   root(관리자, Administrator) / ubuntu
-   `$ su - root (su, switch user : 사용자 전환) Password: ubuntu # ping google.com -- 이 때는 NAT 네트워크 미생성 단계이므로 연결 안됨`

## NAT 네트워크 생성

1.  NatNetwork 생성 및 네트워크 설정한다.
    -   Oracle VM VirtualBox 관리자에서 파일 > 환경 설정
        -   네트워크 --> \[+\]
        -   NatNetwork 선택 --> 톱니바퀴 설정 --> 네트워크 CIDR `192.168.137.0/24` 입력 > 확인

1.  Virtualbox Host-Only ethernet adapter IP 설정 확인
    -   Oracle VM VirtualBox 관리자
        -   도구(3개의 선) > 네트워크 > `Virtualbox host-only ethernet adapter` 선택
        -   하단의 어댑터 탭 선택
            -   IPv4 주소 : 192.168.56.1
            -   IPv4 서브넷 마스크 : 255.255.255.0
2.  가상 머신 네트워크 인터페이스 타입 변경
    -   Oracle VM VirtualBox 관리자 > master 가상 머신 선택 > 네트워크 > 어댑터 1
        -   다음에 연결됨 : `NAT 네트워크` 선택

1.  네트워크 연결 확인  
    리눅스 시스템으로 돌아와서 외부 인터넷과 연결 상태 확인

```bash
    # ping google.com
    [CTRL+C] 
```

-   고정 IP 설정 확인 `cat /etc/netplan/00-installer-config.yaml`

![](/assets/images/dockervb1.png)


-   설정된 IP 확인 ip addr
    


![](/assets/images/dockervb2.png)


## 기타 설정

```bash
server1:~ # echo "kernel.watchdog_thresh=20" > /etc/sysctl.d/99-watchdog_thresh.conf
server1:~ # sysctl -p  /etc/sysctl.d/99-watchdog_thresh.conf
```

## putty 터미널 사용하기

### NatNetwork port-forwarding 설정

Oracle VM VirtualBox 관리자에서 파일 > 환경설정 > 네트워크 > NatNetwork > 톱니바퀴 (설정) > 포트포워딩 > \[+\]

```bash
    이름       프로토콜         호스트IP          호스트 포트           게스트IP                게스트포트
    master  TCP            192.168.56.1       101        192.168.137.101           22
```

이후 putty 접속시 방화벽에서 "액세스 허용" 선택한다.

### putty 설정

putty 사이트에서 다운로드 및 설치 진행 후 putty.exe 실행한다.

```bash
    Host Name : 192.168.56.1    Port: 101

    Terminal > Keyboard > The Backspace key (Control-H)
    Window  > Appearance > Font Settings > Change
    Window > Colours > Default Foreground (글자색)
                                                   Default Background(배경색)

    Session > [Saved Sessions : master] > Save
```

VirtualBox 가 실행된 상태에서 저장된 세션 연결 정보 더블클릭해서 연결 요청을 할 수 있다.

```bash
    [Accept]
    login as : root 
    Password : ubuntu
```

![](/assets/images/dockervb3.png)


## Slave 설정

`06-02-01-01 Kubernetes 실습환경구성` 내용 에서 참조할 것.
---
layout: default
title: SSH 기본 개념
parent: Network & Security
nav_order: 3
---


# SSH 기본 개념
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

시큐어 셀(Secure SHell, SSH)은 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용 프로그램 또는 그 프로토콜이다.

많은 Git 서버들은 SSH 공개키로 인증한다. 공개키를 사용하려면 일단 공개키를 만들어야 한다. 공개키를 만드는 방법은 모든 운영체제가 비슷하다.

## SSH 저장 위치

사용자의 SSH 키들은 기본적으로 사용자의 ~/.ssh 디렉토리에 저장한다. 그래서 만약 디렉토리의 파일을 살펴보면 이미 공개키가 있는지 확인할 수 있다.


{: .highlight }
대표적인 사용 목적으로는 `데이터 전송` 과 `원격제어` 가 있으며, 데이터 전송의 주요 저장소는 깃허브, 원격 저장소 등을 들 수 있다.


소스 코드를 원격 저장소인 깃헙에 푸쉬할 때 SSH를 활용해 파일을 전송하게 된다.

## 원격 제어 

-   AWS와 같은 클라우드 서비스는 인스턴스 서버에 접속하여 해당 머신에 명령을 내리기 위해서 SSH을 통한 접속을 해야한다.사용이유 :
-   보안때문에 FTP나 Telnet과 같은 다른 컴퓨터 통신을 사용하지 않는다.
-   SSH는 보안적으로 훨씬 안전한 채널을 구성한 뒤 정보를 교환하기 때문에 보다 보안적인 면에서 뛰어나다.어떤 방식으로 서로 다른 컴퓨터가 안전하게 통신하게 하는가?SSH는 다른 컴퓨터와 통신할 때 일반적으로 사용하는 비밀번호 입력을 통한 접속을 하지 않는다.  
    - 기본적으로 SSH는 한 쌍의 Key를 통해 접속하려는 컴퓨터와 인증 과정을 거친다. 이 한 쌍의 Key는 다음과 같다.
        - Public Key 
            - 단어 뜻 그대로 공개되어도 비교적 안전한 Key.
            - 이 키를 통해 메세지를 전송하기 전 암호화를 한다.  
            - 암호화는 가능하지만 복호화는 불가능하다.
            
        - Private Key 
            - 절대로 외부에 노출되어서는 안되는 Key.  
            - 본인의 컴퓨터 내부에 저장하게 되어있다.  
            - 암호화된 메세지를 복호화 할 수 있다.
 
## 인증서 생성 예제

```bash
root@master:~# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): testkey <-- 공개키 이름 설정
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in testkey.
Your public key has been saved in testkey.pub.
The key fingerprint is:
SHA256:tN/FZ3XZoZWE0qubqgAUXo1wxRbKEtaQ0ScCRgdrN64 root@master
The key's randomart image is:
+---[RSA 2048]----+
|.=oOBo=o.   . o+.|
|. =o*=.=   . oo.+|
| o *.o+ .   ....+|
|. + o  . .   o  o|
|   o    S   . o o|
|  . .    . o . o |
| E   .    . +    |
|      .    o     |
|       ....      |
+----[SHA256]-----+
root@master:~#


// SSL 인증서 확인

root@master:~#
root@master:~# ls -al | grep test
-rw-------  1 root root   1766 Jan 15 03:56 testkey
-rw-r--r--  1 root root    393 Jan 15 03:56 testkey.pub
root@master:~#

root@master:~# cat testkey
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

root@master:~# cat testkey.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDEmxKWFgQoc3c7vyALV22I+QLSuDxbbEcxuFI/Qz3E0i1yhK23QzXPd8v+Nx7IAbMDVSB3r+OHkEx964jxvptXHFckrFjpYD7QZPiXjTfCEa1CViKmE7CRYBT6qTzZjT368b9JZSOTkr/SOYjin/UBame74wMXTlWeUHTTBf4enVPpLOtOS24wZsmmXGwpNYmhrAnM8PQfoza1WpWnkZN3z7AgpyyYv1eSWk6JGhpsb0b9OsI030TkteFB4H67B0SmH4nZ/60mA3jhehTbPLlD9lHG5nCA+kCbr51YgAXaH4zmwoxi4RBWTcw5WltvSkLk/qxjYoyFUgZA3xOm9AruTNTI191v4NxrHAnN0bJUvi/QdbZBK5ao3FWV2Cgp46KRwCyFR+k4UV1Y7GspTbwruTMOAw42azIpPk4UbbXog6mF7W+/8bqJeHPjYwFtnosk0LZlTGUqhgvB9jT0MYbwRFWUbjByz7pmv7QLhSFHS77/wHGNc3lTWTykxKGeyIE= testuser@testuser-500R5K-501R5K-500R5Q
root@master:~# 



```
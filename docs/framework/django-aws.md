---
layout: default
title: Djanog on AWS EC2
parent: Django
grand_parent: Framework
permalink: /docs/framework/django-aws/
nav_order: 20

--- 


# Djanog on AWS EC2
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


{: .important }
> AWS EC2 서버에서의 서비스를 위해 임대부터 설치까지의 과정을 정리하고자 합니다. `Django` 기준으로 작성하였으며 원격 서버 구축에 놓치기 쉬운 설정들을 나열하였습니다. (`https://aws.amazon.com/`) 

 
 
### `requirements.txt` 파일 생성하기
서버에서 설치하기 위한 패키지들을 `requirements.txt` 에 저장하여  서버에서 간단히 설치할 수 있습니다. manage.py 가 있는 폴더에서 아래와 같이 입력합니다.
```bash
pip freeze >> requirements.txt
```
이후 현재 활성화된 가상환경에 설치된 패키지의 목록을 `requirements.txt` 에 저장할 수 있으며, 아래 명령어로 내용을 확인할 수 있습니다.
```bash
cat requirements.txt
```



### AWS EC2 임대하기
- 로그인 후, 상단의 `서비스` 메뉴를 클릭한 뒤 `EC2` 를 검색하여 들어갑니다.
- 우측 상단의 리전선택에서 `서울` 을 선택합니다.
- 좌측 메뉴의 `인스턴스` 에 들어가 시작을 눌러줍니다.
- 이미지 선택에서 `Ubuntu Server 18.04 LTS` 를 선택합니다.
- 인스턴스 유형에서 `프리티어` 를 선택하면 되는데, 이는 신규 가입자가 1년간 일정할당량을 무료로 사용할 수 있는 서비스로 연습용으로는 적합합니다.
- `인스턴스 시작` 을 눌러줍니다.
이후 `키 페어` 설정 창이 나오는데, 이는 EC2 서버에 원격으로 접속하기 위한 일종의 열쇠라고 이해하시면 되며, 키페어 이름 입력 후 다운로드를 클릭하면 됩니다.
- 노란불과 함께 시작하게 되면 다소 시간이 걸리니 기다려 주시면 됩니다.
- 다운로드 된 키페어(pem) 를 `.ssh` 폴더에 옯깁니다. 외부 컴퓨터와 원격 통신시 `.ssh` 폴더의 키페어를 사용하게 됩니다. 이후 옮겨졌는지 확인합니다.


    ```bash
    $ mkdir ~/ .ssh/
    $ mv ~/Downloads/deploy_test.pem ~/.ssh/

    $ ls ~/.ssh/
    deploy_test.pem
    ```

- 이후 키페어 파일의 권한을 소유주만 읽을 수 있도록 설정을 변경합니다. `chmod` 명령어를 사용하여 읽기, 쓰기, 실행 권한 등을 변경할 수 있습니다.

    ```bash
    $chmod 400 ~/.ssh/deploy_test.pem
    ```

### AWS EC2 서버에 원격 접속하기
배포 전 프로젝트 파일을 원격 서버에 옮기기 위해서는 EC2 서버에 접속해야 합니다. 접속하기 위해서는 `pem` 파일이 필요하여 아래와 같이 원격 접속을 할 수 있습니다.
윈도우 환경에서는 `git bash` 와 같은 cmd 프로그램을 이용하면 수월하게 진행할 수 있습니다.

```bash
$ ssh -i [키페어경로] [유저이름]@[퍼블릭DNS주소]
```

위의 `유저이름` 과 `퍼블릭DNS주소` 는 인스턴스 창에서 확인할 수 있습니다.

- 만약 접속 시 아래와 같은 에러가 발생하면 위의 `chmod 400` 을 적용하였는지 확인하시면 됩니다.
`"WARNING:UNPROTECTED PRIVATE KEY FILE!"`



### AWS EC2 서버 초기 세팅하기

- 임대한 원격서버의 기본 패키지들을 설치합니다. 이전으 ㅣ원격으로 접속한 후에 패키지 정보를 업데이트 합ㄴ디ㅏ.
`$ sudo apt-get update`
뭔가를 물어보는 팝업창이 나타나면 Y 를 클릭하시면 됩니다.

- 이후 패키지 의존성 검사 및 업그레이드를 진행합니다.
``$ sudo apt-get dist-upgrade`

- 이후 python3 패키지 매니저(pip3) 를 설치합니다.
`$ sudo apt-get install python3-pip`

### 프로젝트를 github 에 업로드하기
원격서버에 프로젝트를 옮기기 위해서는 filezilla 와 같은 다양한 프로그램들이 있지만 형상관리를 수월하게 할 수 있는 `github` 를 사용하는 것을 권장합니다.
- `Create a new repository` 를 클릭한 수 프로젝트 명을 입력하고 생성합니다.
- 로컬에서 프로젝트의 `manage.py` 아 있는 곳으로 이동하여 `.git` 폴더을 생성하고 여기에 프로젝트 소스들을 담아 github 에 올리도록 합니다.

  ```bash
  $ git init  # git 을 초기화 함
  $ git add . # 현재 폴더 전체를 담는다.
  $ git remote add origin [레포리토리주소] # 레포지토리주소를 `origin` 이라는 이름으로 추가한다.
  $ git commit -m "first commit" # 변경사항을 모으로 메시지를 입력한다.
  $ git push origin master # origin 이라는 이름의 레포지토리 주소로 업로드 한다.
  ```


이후 변경 사항에 대한 업로드 시에는 아래와 같이 진행할 수 있습니다.

  ```bash
  $ git add .
  $ git commit -m "코멘트"
  $ git push origin master
  ```

### EC2 서버에서 프로젝트 가져오기
`github` 에 있는 프로젝트를 서버로 옮기기 위해 `git clone` 명령어를 사용합니다. 먼저 원격 서버에 접속한 후 아래와 같이 진행합니다.

- 프로젝트 파일을 담을 `/srv/` 폴더의 소유권을 변경합니다. 여기서 '`ubuntu`의 의미는 현재 유저를 가리킵니다.

    ```bash
    $ sudo chown -R ubuntu:ubuntu /srv/

    #소유권 변경 확인 위해 루트 폴더(/) 로 이동한 뒤 아래와 같이 확인합니다.
    $ cd /
    $ ls -al
    ```

- `/srv/` 폴더로 이동하여 github 내 프로젝트를 받아옵니다.

    ```bash
    $ cd /srv
    $ git clone [레포지토리주소]

    # 이후 ls 명령어를 통해 레포지토리 이름과 동일한 폴더가 생성되었다면 성공입니다.

    $ ls

    ```



---
layout: default
title: Djanog on Local
parent: Django
grand_parent: Framework
permalink: /docs/framework/django-local/
nav_order: 15

--- 


# Djanog on Local
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


{: .note }
> 장고 프로젝트를 로컬에서 실행하기 위한 가상 환경 설정 및 모듈 다운로드, 해결책에 대해 정리하고자 합니다. 


## 가상환경 설정
장고 프로젝트를 실행할 가상환경을 구성한다. Python 을 설치한 후, 가상환경 폴더를 구성하고자 하는 폴도로 이동하여 아래와 같이 수행한다.

```bash
D:\1_GitSource\PANABARA>python -m venv pnbr_env

D:\1_GitSource\PANABARA>cd pnbr_env



D:\1_GitSource\PANABARA\pnbr_env>dir
 D 드라이브의 볼륨: 새 볼륨
 볼륨 일련 번호: 7816-659E

 D:\1_GitSource\PANABARA\pnbr_env 디렉터리

2021-11-30  오후 11:01    <DIR>          .
2021-11-30  오후 11:01    <DIR>          ..
2021-11-30  오후 11:01    <DIR>          Include
2021-11-30  오후 11:01    <DIR>          Lib
2021-11-30  오후 11:01               119 pyvenv.cfg
2021-11-30  오후 11:02    <DIR>          Scripts
               1개 파일                 119 바이트
               5개 디렉터리  213,874,466,816 바이트 남음


D:\1_GitSource\PANABARA\pnbr_env>cd Scripts

D:\1_GitSource\PANABARA\pnbr_env\Scripts>activate


(pnbr_env) D:\1_GitSource\PANABARA\pnbr_env\Scripts>


```

`manage.py` 가 있는 폴더로 이동한다.

```bash

(pnbr_env) D:\1_GitSource\PANABARA\pnbr_env\Scripts>cd D:\1_GitSource\PANABARA

// requirements 내 모듈 모두 설치하기
(pnbr_env) D:\1_GitSource\PANABARA>pip install -r requirements.txt

(pnbr_env) D:\1_GitSource\PANABARA>python manage.py makemigrations && python manage.py migrate

(pnbr_env) D:\1_GitSource\PANABARA>python manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

===============BSModalForm started =================
===============BSModalForm started =================
===============BSModalModelForm started =================
===============BSModalModelForm started =================
dddddddddddddddddddddddddddddddddddddddddddddddddd
dddddddddddddddddddddddddddddddddddddddddddddddddd
System check identified no issues (0 silenced).
December 01, 2021 - 21:29:17
Django version 2.2.10, using settings 'setup.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.

```

--- 

## 주요 오류 및 해결 방법

[오류 해결] pip install <패키지> 설치 시 "UnicodeDecodeError: 'cp949' codec can't decode byte 0xe2 in position 2082: illegal multibyte sequence" 에러가 발생하는 경우

- 해당 오류는 pip 명령어로 패키지를 설치하거나 tar.gz, whl 파일로 패키지를 직접 설치할 때 압축 파일 안에 있는 setup.py 파일을 자동으로 실행하는데 여기에 포함되어 있는 코드 중 txt 파일과 같은 것을 읽을 때 디코딩이 제대로 되지 않아서 발생하는 문제이다.
- 또는 특정 패키지가 현재 설치된 Python 버전과 호환되지 않을 수 있으므로, 패키지 버전을 맞추거나 Python 은 다른 버전으로 변경해야 한다.


- 위 문제를 해결하기 위해서는 tar.gz 파일을 받은 후 압축을 해제하여 설치하는 방법을 이용해야 한다. 압축을 해제하면 해당 폴더에는 setup.py 파일이 있을 것이다. 여기 위치에서 cmd 또는 shell을 열어서 직접 python setup.py install 명령어를 실행하여 설치해야 한다. 

Numpy 에러 발생 시
- https://m.blog.naver.com/beacon71/221872094394




---
layout: default
title: Creating Django 
parent: Django
grand_parent: Framework
permalink: /docs/framework/django-project/
nav_order: 10

--- 


# Creating Django 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---



## Dango 서버 구축


- 폴더를 만든다. 
`mkdir djangotest`

- vscode 에서 폴더열기를 한다.

- 이후 터미널을 실행한다.

- 가상환경을 만든다.
    `python -m venv venv`

- f1 을 눌러 SelectorInterpreter 에서 ` Python: Select Interpreter` 를 선택한다.

- 생성한 가상환경 내 `../Script/python.exe` 를 선택한다.

	- 해당 파일이 없다면 종료후 다시 실행한다.

- 터미널을 실행하여 우측상단 + 버튼을 클릭하여 가상환경을 activate 한다.

	- activate.ps1 파일을 로드할 수 없습니다 오류 시 파워쉘 관리자에서 아래를 실행한다.

		`PS C:\WINDOWS\System32> Set-ExecutionPolicy Unrestricted`


- 장고를 설치한다.

```bash
(venv) PS D:\5_Django\djangotest> pip list     
Package    Version
---------- -------
pip        18.1
setuptools 40.6.2
```

```bash
(venv) PS D:\5_Django\djangotest> pip install django
```

```bash
(venv) PS D:\5_Django\djangotest\FirstProject\firstproject> pip list
Package           Version
----------------- -------
asgiref           3.4.1
Django            3.2.13
pip               18.1
pytz              2022.1
setuptools        40.6.2
sqlparse          0.4.2
typing-extensions 4.1.1
```

- 가상환경의 상위 폴더에 프로젝트를 생성한다.

```bash
(venv) PS D:\5_Django\djangotest> mkdir FirstProject
(venv) PS D:\5_Django\djangotest> django-admin.exe startproject firstproject
```


- 프로젝트로 이동한 후 동작을 실행하도록 하겠습니다. 먼저 makemigrations 와 migrate를 입력해 프로젝트의 변동사항을 데이터베이스에 적용시킵니다.

```bash
(venv) PS D:\5_Django\djangotest\FirstProject> cd .\firstproject\        
(venv) PS D:\5_Django\djangotest\FirstProject\firstproject> python manage.py makemigrations
No changes detected
(venv) PS D:\5_Django\djangotest\FirstProject\firstproject> python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
```


- python manage.py runserver를 입력해 서버를 실행시킵니다.

```bash
(venv) PS D:\5_Django\djangotest\FirstProject\firstproject> python manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
April 21, 2022 - 23:01:52
Django version 3.2.13, using settings 'firstproject.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
[21/Apr/2022 23:01:54] "GET / HTTP/1.1" 200 10697
```


- 앱만들기 

`(venv) PS D:\5_Django\quiz_backend> python manage.py startapp quiz`


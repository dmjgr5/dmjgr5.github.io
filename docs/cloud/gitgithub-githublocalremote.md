---
layout: default
title: Github 로컬/원격 연동
parent: Git & Github
grand_parent: Cloud
permalink: /docs/cloud/gitgithub-githublocalremote/
nav_order: 50
---

# Github 원격 저장소와 로컬 저장소 연동하기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .note }
로컬에 있는 데이터 및 소스들을 Github 에 올리기 위해 필요한 절차 및 명령어를 간단히 정리해 보고자 한다. 기본적인 내용이지만 가끔 생성할 때마다 기억이 잘 안나 구글링을 다시 해보곤 해서 이참에 정리하고자 한다.
 


## 1. Local Directory 생성

![generateFolder](/assets/images/gitset1.jpg)

- 위 C:\gitShare 폴더에 Github 에 올리고자하는 폴더 및 파일들이 있다고 가정하다. 먼저 gitBash 에서 아래와 같이 .git repository 를 생성하기 위해 아래 명령어가 필요하다.

```shell
dc@ MINGW64 /c/gitShare
$ git init
```


- 아래와 같이 .git 디렉토리가 생성된 것을 확인할 수 있다.
![generateFolder](/assets/images/gitset2.jpg)


------

## 2. Git Repository 생성

- Github 에서 push 할 저장소인 원격 repository 를 아래 메뉴를 통해 생성한다.
![generateFolder](/assets/images/gitset3.jpg)

- Repository 생성 후, 아래 주소를 복사한다.
![generateFolder](/assets/images/gitset4.jpg)


------


## 3. Github 연결 설정

```shell
dc@ MINGW64 /c/gitShare (master) 
$ git remote add origin [git address]
```

- 위와 같이 복사한 주소를 git remote add origin [복사 주소] 명령어를 통해 Local Repository 를 원격 저장소와 동기화 한다.
필요 시, Github 계정을 통해 인증한다.



------

## 4. Commit and Push

- Working directory 데이터를 git Repository 로 옮기기 위해 아래 명령이 필요하다.

```shell
dc@ MINGW64 /c/gitShare (master)
$ git add .

dc@ MINGW64 /c/gitShare (master)
$ git commit -m "init"
[master (root-commit) 97d2ae3] init
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt
```


- 이후 Push 명령어를 통해 Github repository 에 올릴 수 있다.

```shell
dc@MINGW64 /c/gitShare (master)
$ git push origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 203 bytes | 67.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/dmjgr5/test/pull/new/master
remote:
To https://github.com/dmjgr5/test.git
 * [new branch]      master -> master
```

- Github 에서 아래와 같이 해당 데이터가 올라온 것을 확인할 수 있다.

![generateFolder](/assets/images/gitset5.jpg)



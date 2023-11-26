---
layout: default
title: Set Me Up for Remote Repository
parent: Jfrog
grand_parent: CI/CD Tools 
permalink: /docs/cicdtools/jfrog-setmeuprepo/
nav_order: 35
---

# Set Me Up for Remote Repository
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


{: .note }
Jfrog 에서의 package 관리를 위한 생성 및 각각의 특징에 대해 설명한다.

## Docker



### PULL from Remote Docker Hub to Remote Repository


```bash
## login

root@jfrog-ubuntu-18-04:~# docker login -utestuser@naver.com testuser.jfrog.io
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See

Login Succeeded
root@jfrog-ubuntu-18-04:~# docker image ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

## pull : docker pull testuser.jfrog.io/docker-demo-remote/<DOCKER_IMAGE>:<DOCKER_TAG>

root@jfrog-ubuntu-18-04:~# docker pull testuser.jfrog.io/docker-demo-remote/nginx:1.9.5

root@jfrog-ubuntu-18-04:~# docker pull testuser.jfrog.io/docker-demo-remote/nginx:latest
latest: Pulling from docker-demo-remote/nginx
f03b40093957: Pull complete 
eed12bbd6494: Pull complete 
fa7eb8c8eee8: Pull complete 
7ff3b2b12318: Pull complete 
0f67c7de5f2c: Pull complete 
831f51541d38: Pull complete 
Digest: sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305
Status: Downloaded newer image for testuser.jfrog.io/docker-demo-remote/nginx:latest
testuser.jfrog.io/docker-demo-remote/nginx:latest
```

### PUSH Image from Local to Local Repository

```bash
## login

root@jfrog-ubuntu-18-04:~# docker login -utestuser@naver.com testuser.jfrog.io
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded


root@jfrog-ubuntu-18-04:~# docker image ls
REPOSITORY                                       TAG       IMAGE ID       CREATED          SIZE
alpine-test                                      1.0       5fd6cce2f848   26 minutes ago   5.58MB

## TAGGING

root@jfrog-ubuntu-18-04:~# 
root@jfrog-ubuntu-18-04:~# 
root@jfrog-ubuntu-18-04:~# docker tag alpine-test:1.0 testuser.jfrog.io/docker-demo-local/alpine-test:1.2
root@jfrog-ubuntu-18-04:~# docker image ls
REPOSITORY                                       TAG       IMAGE ID       CREATED          SIZE
testuser.jfrog.io/docker-demo-remote/alpine-test   1.1       5fd6cce2f848   27 minutes ago   5.58MB
alpine-test                                      1.0       5fd6cce2f848   27 minutes ago   5.58MB


## push

root@jfrog-ubuntu-18-04:~# 
root@jfrog-ubuntu-18-04:~# docker push testuser.jfrog.io/docker-demo-local/alpine-test:1.2 
The push refers to repository [testuser.jfrog.io/docker-demo-local/alpine-test]
9fb3aa2f8b80: Pushed 
1.2: digest: sha256:5748740b6920bdcf312855a936d9984acf7c0ff38a62a43a3cbd30275a30fca6 size: 528
root@jfrog-ubuntu-18-04:~# 

```




## Maven

### settings.xml in .m2 folder 

### pom.xml 디펜던시 리스트

### 07-02-03 Build and Deploy Artifacts to Jfrog  자료 참조

## npm

### 사전준비

.npmrc 에 아래 추가

```bash
email=testuser@naver.com
always-auth=true
registry=https://testuser.jfrog.io/artifactory/api/npm/default-demo-npm/
//testuser.jfrog.io/artifactory/api/npm/default-demo-npm/:_auth="ZGNwYXJrODBAbmF2ZXIuY29tOkFQNmpHS2dTaTdrYmtlOVRBcGlLU25UaTFaTA=="
```

### PULL from remote NPM repository

```bash
npm install mathjs@^9.4.4
```

## Pypi

### 사전준비

1. pip.ini 파일 global 로딩 위치 확인하기

```bash
C:\> pip config -v list
For variant 'global', will try loading 'C:\ProgramData\pip\pip.ini'
For variant 'user', will try loading 'C:\Users\**\pip\pip.ini'
For variant 'user', will try loading 'C:\Users\**\AppData\Roaming\pip\pip.ini'
For variant 'site', will try loading 'c:\python39\pip.ini'
```

2. pip.ini 파일 만들기

```bash
C:\>mkdir C:\ProgramData\pip
C:\>cd C:\ProgramData\pip
C:\ProgramData\pip> code pip.ini
```

3. pip.ini 파일 작성

```bash
[global]
index-url = https://testuser@naver.com:cmVmdGtuOjAxOjE3MTY1NTgzMDA6bkVKcEZ0ZVJJSTRPZ1JHdkxiN1FwMFNnWnlB@testuser.jfrog.io/artifactory/api/pypi/default-demo-pypi-remote/simple
```
               

### INSTALL from Remote Repository

```bash
C:\Users\dcjam\AppData\Local\Programs\Python\Python311>pip install tensorflow
C:\Users\dcjam\AppData\Local\Programs\Python\Python311>pip install requests==2.24.0
C:\Users\dcjam\AppData\Local\Programs\Python\Python311>pip install nltk==3.6.7
```

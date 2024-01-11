---
layout: default
title: Set Me Up for Remote Repository
parent: Jfrog
grand_parent: CI/CD Tools 
permalink: /docs/cicdtools/jfrog-setmeuprepo
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



### docker login


```bash
## login

root@jfrog-ubuntu-18-04:~# docker login -utestuser@naver.com testuser.jfrog.io
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See

Login Succeeded
root@jfrog-ubuntu-18-04:~# docker image ls
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

### docker pull

pull : docker pull testuser.jfrog.io/docker-demo-remote/<DOCKER_IMAGE>:<DOCKER_TAG>

```sh
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

### docker tag

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

```

### docker push

```sh
root@jfrog-ubuntu-18-04:~# 
root@jfrog-ubuntu-18-04:~# docker push testuser.jfrog.io/docker-demo-local/alpine-test:1.2 
The push refers to repository [testuser.jfrog.io/docker-demo-local/alpine-test]
9fb3aa2f8b80: Pushed 
1.2: digest: sha256:5748740b6920bdcf312855a936d9984acf7c0ff38a62a43a3cbd30275a30fca6 size: 528
root@jfrog-ubuntu-18-04:~# 

```

## Helm Chart 

### helm repo add


```sh
user1@user1-500R5K-501R5K-500R5Q:~/.npm$ helm repo add mytest-helm https://dmjgr13.jfrog.io/artifactory/api/helm/mytest-helm --username dmjgr13@gmail.com --password cmVmdGtuOjAxOjE3MzUwMzYyNDE6am9saUxLYnFSeXRDekN5Wll0amRrQ0FoVEhs 
```

### helm repo list 

```sh
user1@user1-500R5K-501R5K-500R5Q:~/.npm$ helm repo list
NAME         	URL                                                      
stable       	https://charts.helm.sh/stable                            
ingress-nginx	https://kubernetes.github.io/ingress-nginx               
mytest-helm  	https://dmjgr13.jfrog.io/artifactory/api/helm/mytest-helm
```


### helm package {tgz}

```sh
user1@user1-500R5K-501R5K-500R5Q:~$ ls | grep hello
hello-world
user1@user1-500R5K-501R5K-500R5Q:~$ helm package hello-world/
Successfully packaged chart and saved it to: /home/user1/hello-world-0.1.0.tgz

user1@user1-500R5K-501R5K-500R5Q:~$ 
user1@user1-500R5K-501R5K-500R5Q:~$ ls | grep hello
hello-world
hello-world-0.1.0.tgz
user1@user1-500R5K-501R5K-500R5Q:~$ 
```

### upload to repository

```sh
user1@user1-500R5K-501R5K-500R5Q:~$ curl -udmjgr13@gmail.com:cmVmdGtuOjAxOjE3MzUwMzYyNDE6am9saUxLYnFSeXRDekN5Wll0amRrQ0FoVEhs -T hello-world-0.1.0.tgz "https://dmjgr13.jfrog.io/artifactory/mytest-helm/hello-world-upload"
{
  "repo" : "mytest-helm-local",
  "path" : "/hello-world-upload",
  "created" : "2023-12-25T10:40:01.969Z",
  "createdBy" : "dmjgr13@gmail.com",
  "downloadUri" : "https://dmjgr13.jfrog.io/artifactory/mytest-helm-local/hello-world-upload",
  "mimeType" : "application/octet-stream",
  "size" : "2391",
  "checksums" : {
    "sha1" : "6f10a1334574f55c232627ec03adb07d77b27697",
    "md5" : "abb4b8514f5f4421969e6f86fbe80bcf",
    "sha256" : "7780b41d0839f48c28b7e5aaa24931bab27c5a9742f3580026b69586d4bd0672"
  },
  "originalChecksums" : {
    "sha256" : "7780b41d0839f48c28b7e5aaa24931bab27c5a9742f3580026b69586d4bd0672"
  },
  "uri" : "https://dmjgr13.jfrog.io/artifactory/mytest-helm-local/hello-world-upload"
}user1@user1-500R5K-501R5K-500R5Q:~$ 
```

## Maven

### settings.xml in .m2 folder 

### pom.xml 디펜던시 리스트

### 07-02-03 Build and Deploy Artifacts to Jfrog  자료 참조





## npm

### npmrc 설정

#### npm config set registry


.npmrc 에 아래 추가

```sh
user1@user1-500R5K-501R5K-500R5Q:~/.npm$ npm config set registry https://dmjgr13.jfrog.io/artifactory/api/npm/testnpm/
user1@user1-500R5K-501R5K-500R5Q:~/.npm$ cat .npmrc 
registry=https://dmjgr13.jfrog.io/artifactory/api/npm/dctest-npm/
_auth=ZG1qZ3IxM0BnbWFpbC5jb206Y21WbWRHdHVPakF4T2pFM016UTFNalF5TmprNlZIWTNjVXh5YXpCa2NraDJUbEYzWVVoamNVRkpPVFkzUkZOQg==
always-auth=true
```


{: .note}
> `npm config get userconfig` 명령어를 이용하여 npmrc 파일 위치를 확인할 수 있다. `-v` 옵션은 자세한 사항을 조회할 수 있다.
 

#### npmrc 직접 설정

```sh
registry=https://registry.npmjs.org/ --> default registry
@myscope:registry=http://nexusdomain.com/nexus/content/repositories/npm-test/ --> 추가 registry
//nexusdomain.com/nexus/content/repositories/npm-test/:username=admin --> username
//nexusdomain.com/nexus/content/repositories/npm-test/:_password=YWRtaW4xMjM=  --> password
//nexusdomain.com/nexus/content/repositories/npm-test/:always-auth=true --> 인증이 필요할 경우 설정한다.
email=…
noproxy[nexusdomain.com] --> 특정 도메인에 대해 proxy 를 skip 할 경우 세팅한다.
```

_password 대신 _auth 또는 _authToken 을 사용할 수 있다.

- _auth : `username:password` 를 base64 로 암호화한 값이 들어간다.
- _authToken : user token 값이 들어간다.





#### 1. package.json 

scope 기준으로 생성하기 위해서는 아래와 같이 name 에 @ 와 함께 scope/ 명을 추가한다.

```json
{
  "name": "@myscope/sample",
  "version": "1.0.0",
  "description": "description message",
  "main": "index.js",
  "author": "author info"
}
```

#### 2. index.js

```javascript
module.exports = function() {
  console.log("you're wonderful!");
  return;
};
```

#### 3. README

```sh
readme content
```

위 파일을 생성한 후 publish 를 진행한다.



### npm publish
 

```sh
npm publish
```


### npm cache clean --force

npmrc 를 변경했을 경우, 이전 캐쉬정보가 남아있어 변경된 registry 나 인증 정보들이 갱신되지 않을 경우가 있다. 이 경우에는 `npm cache clean --force` 를 먼저 수행하자.


### npm install

#### 1. package.json 을 통한 npm install

package.json 을 참조하여 install 할 경우에 scope 참조하는 npm package 들은 package.json 에 아래와 같이 scope 를 명시해야 한다.

```json
{
    "dependencies": {
      "@myscope/sample": "^1.0.0"
    }
}
```

이후 `npm install` 을 통하여 패키지를 다운로드 받을 수 있다.

#### 2. 특정 패키지에 대한 npm install

`@myscope` 에 대한 registry 를 npmrc 에 선언했을 경우 아래와 같다.

```bash
npm install @myscope/sample@1.0.0
```

또는 registry 를 명시적으로 선언할 수 있다.


```bash
 npm install myscope@1.1.0 --registry http://nexusdomain.com/nexus/content/repositories/npm-test/
 ```

## Pypi

### setting pip.ini

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
               

### pip install

```bash
C:\Users\dcjam\AppData\Local\Programs\Python\Python311>pip install tensorflow
C:\Users\dcjam\AppData\Local\Programs\Python\Python311>pip install requests==2.24.0
C:\Users\dcjam\AppData\Local\Programs\Python\Python311>pip install nltk==3.6.7
```

---
layout: default
title: npm
parent: Build & Package
nav_order: 15
--- 

# Maven 생성 및 실행
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## npmrc 설정

### npm config set registry  명령어로 설정


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
 

### npmrc 직접 설정

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


## 실행 파일 만들기

npm 패키지 생성을 위해서는 최소 아래의 파일이 필요하다.

### 1. package.json 

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

### 2. index.js

```javascript
module.exports = function() {
  console.log("you're wonderful!");
  return;
};
```

### 3. README

```sh
readme content
```

위 파일을 생성한 후 publish 를 진행한다.



## npm publish
 

```sh
npm publish
```


## npm cache clean --force

npmrc 를 변경했을 경우, 이전 캐쉬정보가 남아있어 변경된 registry 나 인증 정보들이 갱신되지 않을 경우가 있다. 이 경우에는 `npm cache clean --force` 를 먼저 수행하자.


## npm install

### 1. package.json 을 통한 npm install

package.json 을 참조하여 install 할 경우에 scope 참조하는 npm package 들은 package.json 에 아래와 같이 scope 를 명시해야 한다.

```json
{
    "dependencies": {
      "@myscope/sample": "^1.0.0"
    }
}
```

이후 `npm install` 을 통하여 패키지를 다운로드 받을 수 있다.

### 2. 특정 패키지에 대한 npm install

`@myscope` 에 대한 registry 를 npmrc 에 선언했을 경우 아래와 같다.

```bash
npm install @myscope/sample@1.0.0
```

또는 registry 를 명시적으로 선언할 수 있다.


```bash
 npm install myscope@1.1.0 --registry http://nexusdomain.com/nexus/content/repositories/npm-test/
 ```

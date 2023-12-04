---
layout: default
title: Docker Login 사용법
parent: Docker
grand_parent: Build & Package
permalink: /docs/buildpackage/docker-imagelayer/
nav_order: 35
---

# Docker Login 사용법
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
Docker 이미지를 push, pull 하기 위해서는 저장소에 접속하여야 한다. docker.io 공식 저장소 이외에도 docker image 를 저장할 수 있는 여러 공식 및 사설 저장소가 존재한다. 여러 저장소에서 push, pull 을 위해 매번 login 하는 번거로움을 줄일 수 있다.


## 일반적인 login 

```sh
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ docker login myid.jfrog.io
Username: myid@gmail.com
Password: 
WARNING! Your password will be stored unencrypted in /home/user1/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ 
```

## ~/.docker/config.json

최초 로그인 시 출력된 경고 문구를 자세히 보자면 config.json 에 base64 인코딩으로 저장됨을 알 수 있다.

`Your password will be stored unencrypted in /home/user1/.docker/config.json.`

한번 들여다 보자.

```sh
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ cat config.json 
{
	"auths": {
		"myid.jfrog.io": {
			"auth": "bbbbbbbbbbbbbbbbbbbbbbbbbbbb="
		},
		"https://index.docker.io/v1/": {
			"auth": "ccccccccccccccccccccccccc=="
		}
	}
}
user1@user1-500R5K-501R5K-500R
```

방금 전에 로그인한 계정/패스워드가 인코딩되어 있다.



## docker push

그럼 이후부터 push 를 하면 어떻게 될까? 당연히 푸쉬가 가능하다.

```sh
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ docker push myid.jfrog.io/spring-vue-starter-repo/spring-vue-image:131
The push refers to repository [myid.jfrog.io/spring-vue-starter-repo/spring-vue-image]
5e870a0d406b: Pushed 
1ff8e2002eb8: Pushed 
7b7f3078e1db: Pushed 
826c3ddbb29c: Pushed 
b626401ef603: Pushed 
9b55156abf26: Pushed 
293d5db30c9f: Pushed 
03127cdb479b: Pushed 
9c742cd6c7a5: Pushed 
131: digest: sha256:7937aa480bd88dddddddddddddddddddd2988fc0e33800864f40a364 size: 2213
```

## docker logout myid.jfrog.io

docker logout 을 하니 config.json 파일에서 삭제가 된다.

```sh
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ docker logout myid.jfrog.io
Removing login credentials for myid.jfrog.io
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ cat config.json 
{
	"auths": {
		"https://index.docker.io/v1/": {
			"auth": "ccccccccccccccccccccccccc=="
		}
	}
}
user1@user1-500R5K-501R5K-500R
```

## docker push

이 상태에서 pull 을 해보자.

```sh
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ docker push myid.jfrog.io/spring-vue-starter-repo/spring-vue-image:132
The push refers to repository [myid.jfrog.io/spring-vue-starter-repo/spring-vue-image]
5e870a0d406b: Retrying in 1 second 
1ff8e2002eb8: Retrying in 1 second 
7b7f3078e1db: Retrying in 1 second 
826c3ddbb29c: Retrying in 1 second 
b626401ef603: Retrying in 1 second 
9b55156abf26: Waiting 
293d5db30c9f: Waiting 
03127cdb479b: Waiting 
9c742cd6c7a5: Waiting 
unknown: Authentication is required
user1@user1-500R5K-501R5K-500R5Q:~/.docker$ 
```

retry 시도 후 인증이 필요하다는 메시지가 나온다.

## 결론

docker login 을 실행하면 `~/.docker/config.json` 파일에 계정 인증 정보가 base64 로 인코딩 되어 저장되고, 이후 이 데이터는 유지된다. 유지된다는 말은 이후에도 login 절차 없이 인증 정보를 이용하여 해당 저장소로 접근 가능하다는 의미이다. 반대로 docker logout 을 실행하면 logout 과 함계 config.json 파일에 해당 인증 정보가 사라지며 이후에는 해당 저장소로 접근이 불가능해진다.

[https://stackoverflow.com/questions/36022892/how-to-know-if-docker-is-already-logged-in-to-a-docker-registry-server](https://stackoverflow.com/questions/36022892/how-to-know-if-docker-is-already-logged-in-to-a-docker-registry-server)

위 링크에서도 로그인 관련해서 문답이 있다. 그 중 아래 문구가 확실히 눈에 띈다.

{: .important}
docker login actually isn't creating any sort of persistent session, it is only storing the user's credentials on disk so that when authentication is required it can read them to login






---
layout: default
title: Nginx 설정
parent: Network & Security
nav_order: 3
---


# Nginx 설정
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
{: .note}
Nginx 기본 설정에 대해 알아보자.


## 사전 준비
테스트용 서버로 간단한 springboot 로 구성된 jar 파일을 실행 시킨 후 접속을 확인한다. 포트는 `8888` 로 설정하였다.

![](/assets/images/nginx8888.png)

*[localhost:8888 접속]*
{: .text-center }

##  nginx 설치하기

아래 명령어를 이용하여 nginx 를 설치한다.

```sh
sudo apt-get install nginx
```

## 설정 변경하기

`/etc/nginx/sites-available` 폴더의 defalut 파일 내의 내용을 모두 삭제한 후 아래와 같이 변경한다.
`servername:listen` 주소로 접근 시 `proxy_pass` 에 위치한 서버의 `/` 위치로 프록시를 한다.

```sh
root@user1-500R5K-501R5K-500R5Q:/etc/nginx/sites-available# cat default 
#아래는 로드 밸런싱 시 추가
#upstream backend {
#    server localhost:8080;
#    server localhost:8081;
#    server localhost:8082;
#}

server {
    listen 8887;
    server_name localhost;

    location / {
        proxy_pass http://localhost:8888;
    }
}
```
## 서비스 재시작

```sh
sudo service nginx restart
```

## 리버시 프록시 확인

서비스 재시작 후 `8887` 포트로 접근하면 아래와 같이 proxy 서버로 연결됨을 확인할 수 있다.


![](/assets/images/nginx8887.png)

*[localhost:8887 접속]*
{: .text-center }

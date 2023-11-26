---
layout: default
title: Docker Architecture
parent: Docker
grand_parent: Build & Package
permalink: /docs/buildpackage/docker-arch/
nav_order: 21
---

# Docker Architecture

---


{: .note }
Docker 는 2010 년 docCloud 회사로 설립하였으며, 2013 년에 Docker 회사명 변경하였다.

![](/assets/images/dockerarch1.jpeg)

*[Docker 구조 ]*
{: .text-center }

- `Docker Daemon` : `dockerd` 라는 이름의 데몬프로그램, 컨테이너 내에 설치되며 도커 서버의 역할을 한다.  

- `Docker Client` : dockerCLI 이며 명령어를 보내는 작업을 일컫는다.  

- `Registries` : 중앙 이미지 저장소, 도커가 운영하는 퍼블릭 레지스트리 ([https://hub.docker.com](https://hub.docker.com))  

- `Image` : `Docker Container` 를 생성할 때 필요한 읽기 전용의 템플릿, 파일들의 모음  
> `docker.io/myrepo/centos:1.0` : 각 순서대로 레지스트리, 저장소, 이미지, 태그를 일컫는다.  

- `Container` : 격리된 실행 환경, 이미지를 기반으로 실행된 응용프로그램의 프로세스  
\- 읽기 전용의 `image` 에 변경된 사항을 컨테이너 계층(`Container Layer`) 즉, `upperdir` 에 저장하므로 `image` 의 변함은 없다.


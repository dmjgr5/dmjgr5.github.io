---
layout: default
title: AWS Basic
parent: AWS
grand_parent: Cloud
permalink: /docs/cloud/aws-basic/
nav_order: 10
---

# AWS Basic
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

Kubernetes 를 AWS 에 구축하기 위한 기본적은 절차와 고려해야 할만한 사항들을 간략히 정리해 보았습니다. 이해하기 쉬운 내용으로 작성하였으며, 보다 자세한 내용은 구글링을 통해 확인하면 좋을 것 같습니다.



## AWS 의 시작

EC2 에 tomcat/apache 설치하고 war 파일을 넣으면 돌아갈까?
- 일부 설치파일도 필요하다.
- OS/jdk/tomcat 등의 버전 확인도 필요하다.

## AWS Console 계정 생성하고 설치해보자
- 대규모의 사이트의 경우는 과금도 고려해야 한다.
- 지역/ 사용할 네트워크/ Subnet 등을 고려해야 한다.
- 근래에는 elastic search, kafka 등의 오픈 소스 설치도 고려해야 한다.
- AWS 는 RDS 를 제공하기 때문에 별도 DB 생성하지 않아도 된다.

{: .note-title }
> Container 란?
>
>- 여러 설치를 하나의 가상 공간에 설치하여 가상 공간을 서버와 연결하게 하는 역할을 해준다.
>- Container 내 여러 모듈들이 상호작용을 해야하므로 운영자가 필요하다. 이는 ㅏubernetes 가 담당할 것이다.
>- AMS 의 경우, 이는 EKS 모듈을 사용하면 수월하게 관리할 수 있게 해준다.

 
 
## 주요 내용

### AWS 리소스 만들기
계정은 만들어 있으니 EC2 인스턴스를 만들어서 리전을 선택한다. 그리고 나만의 Network (VPC) 를 만들고 Subnet 을 만든다.
OS 선택하고, 서버 사양을 설정하고 방화벽도 설정하고(Security Group), Keypair 도 설정해야 한다.
> Security Group 은 EC2 자체에서도 아무거나 접속하지 못하게 한다.


### Container 만들고 띄워보기
docker 를 설치했는데 뭐부터 해야할까?
OS docker image 를 받은 후, Docker image 안에 들어가 본다.

```bash
docker run -it ...
```

이후 jdk 와 같은 여러가지 설치해본다.
이러한 작업은 어렵고 복잡하기 때문에 Dockerfile 을 만들어 실행한다. 대략적인 형식은 아래와 같다.

```bash
FROM...
RUN ...
COPY ...
RUN ...
USER ...
EXPOSE 8709 ...
ENTRYPOINT ...
```
   
   
   
이후 build , push 한다.

```bash
docker build -tag
docker push
```

### EKS 설치하기
k8s 의 설치와 관리 부담을 줄이기 위하여 사용할 수 있다.
- 권한(IAM), 설치, 관리(kubectl), 등등의 작업을 AWS EKS 에서 제공한다. 이후 EKS 클러스터 생성하고 워크노드를 생성한다.


 
### yaml 파일을 만들면 진짜 동작할까?
내가 만든 container 를 진짜 kubernetes 에서 작동할 수 있을까?
- namespace 를 만들어 보자.(파일의 폴더처럼..)
    - ex) deployment.yaml, service.yaml

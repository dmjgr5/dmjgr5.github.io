---
layout: default
title: Kubernetes 실습 환경 - Minikube 설치
parent: Kubernetes
grand_parent: Cloud
permalink: /docs/cloud/k8s-install-lab-minikube/
nav_order: 6
---

# Kubernetes 실습 환경 - Minikube 설치
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .highlight }
로컬에서 테스트를 위해 간단히 설치하고 싶을 경우엔 Minikube 가 유용하다. 아래 블로그를 참조하여 작성하였다.
- `https://www.joinc.co.kr/w/man/12/kubernetes/minikube`




## MINIKUBE 설치 


```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## CLUSTER 시작

```sh
minikube start
```

## CLUSTER 확인 및 kubectl 설치

```sh
minikube kubectl -- get pods -A
```

## alias 설정

```sh
alias kubectl="minikube kubectl --"
```

## Minikube 사용법

아래를 참조하여 진행해 보자.

`https://www.joinc.co.kr/w/man/12/kubernetes/minikube`

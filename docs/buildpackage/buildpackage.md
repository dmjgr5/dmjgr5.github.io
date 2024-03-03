---
layout: default
title: Build & Package
nav_order: 60
has_children: true
permalink: /docs/buildpackage
---

# Build & Package

소프트웨어 개발 이후 배포를 위한 build tool 과 그와 관련한 각 패키지의 종류 및 특징을 설명한다.
{: .fs-6 .fw-300 }


## npm 

- setting .npmrc
- setting package.json & *.js
- npm publish
- npm cache clean –force
- npm install

## mvn
- setting pom.xml
- mvn clean
- mvn compile
- mvn package 
- mvn install
- mvn deploy

## python 
- 가입 : https://pypi.org/ , dmjgr5/aa1111@aa


- python -m pip install --upgrade build //빌드 모듈 설치
- python -m build // 빌드 하기(wheel, tgz)

- python -m pip install --upgrade twine // 업로드 모듈 설치
- python -m twine upload dist/* // 업로드

- setting pip.ini
- pip install

## docker 

- docker login 
- docker build
- docker tag
- docker push 
- docker pull

## helm 
- helm repo add
- helm repo list
- helm repo search
- helm package
- curl 
- helm pull ??
- helm install


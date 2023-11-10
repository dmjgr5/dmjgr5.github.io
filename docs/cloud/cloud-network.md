---
layout: default
title: Cloud Network
parent: Cloud
# has_children: true
nav_order: 5
---



# Cloud Network
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}




{: .highlight }
기업의 핵심 경쟁력으로 Digital Transformation 이 화두가 됨에 따라 클라우드 컴퓨팅 기술이 대세인 시대입니다. 대표적인 기업으로 넷플릭스는 클라우드 기반의 데이터 센터를 구축하기도 했습니다. 네트워크 구조로서 IaaS, PaaS, SaaS 순으로 변하는 경향이 있으며 클라우드 네트워크의 주요 내용을 정리해 보고자 합니다. 


---

![example](/assets/images/cloudcomputing.png)
 

## Provider 종류
- CSP : (Cloud Service Provider) :AWS(32%), Azure(20%), GCP(9%) 2021 년 기준
- MSP : (Managed Service Provider):운영 및 기술지원 , ex) 메가존, SK C&C, LG C&S, SDS
- ISP : (Independent Software Provider)

![example](/assets/images/cloudms.png)


## 서비스 유형
- IaaS :인프라 제공
- PaaS :응용 프로그램 개발 시 플랫폼 제공
- SaaS :소프트웨어 제공(개발 불필요) ex)Google WorkSpace

## 클라우드 서비스 모델
- Private : 기업 민감 정보, 기밀 정보 운용(ex 은행, 공공기관)
- Public : 클라우드 일반적인 유형 , 서버/스토리지 운용 및 제공, 요금만 지불, 인프라 운용에 효율적
- Hybrid : 혼합, 기밀정보 private, 웹서버 퍼블릭 등의 구분하여 혼합하여 제공 가능
- Multi : 2개 이상 클라우드로 구축하며, 서비스 가용성을 높이고, 장애시 백업 클라우드를 운용하여 장애를 방지할 수 있다. 또한 요구수준에 따라서 유연하고 복수의 제공업체로부터 서비스가 가능하다.

## Cloud Native 모델
- 클라우드의 이점을 활용할 수 있도록 어플리케이션 구축/운용
- DevOps 개념과 실제 서비스를 하나의 프로세스로 자동화하는 방법론
- 컨테이너/ MSA 기술 필요 : 경쟁 우위 확보, 유연성, 개발에 집중, 비즈니스에 집중

## Cloud Native 4 가지 핵심 요소


![example](/assets/images/cloudnative.png)

`Cloud Native` 는  프라이빗, 퍼블릭 및 하이브리드 클라우드 환경 전체에 지속적인 개발과 자동화된 관리 환경을 제공하기 위해 특별히 설계된 애플리케이션을 의미합니다.

- Containers : 운영체체를 가상화한 개념
- MSA : 실행이 가능한 어플리케이션 단위 프로세스
- DevOps :자동화 툴을 이용해 효율적인 프로젝트 관리
- CI/CD : DevOps 를 구현하기 위한 일련의 프로세스 (Continuous Integration/ Continuous Distribution )


---- 


### 컨테이너
- 소스 개발 -> 이미지 파일 -> 컨테이너 실행 (도커:라이브러리, 실행파일 포함 -> 어플리케이션 실행)
- 이식성 :이미지에 어플리케이션에 필요한 모든 것을 넣을 수 있으며 확장이 용이하다.
- 쿠버네티스 (k8s) 의 필요성 : 컨테이너 관리를 여러 기능들을 통해 자동화한다. 스케줄러 통해 리소스 체크/ 컨테이너 자동 생성,제거)
    - 아마존 :EKS, 구글 :Anthos, MS:Azure

### MSA
- Loose Coupling : 느슨한 결합, 독립적인 개발과 운영
- 서비스 간 통신이 많으므로 Service Mesh 로 구성한다. 데이터 결로의 적절한 설정, 프록시 추가 (장애전파 방지, 부하 분산)
- 명확한 목적성을 가지고 도입 고려해야 한다.
- 모놀리틱 : 하나의 전체 서비스 , 기존의 서비스 아키텍쳐
    - 구조 간단, 빠른 개발 가능, 규모가 커지면 개발, 서비스 어려움, 개발,테스트 속도 느려짐
- MSA: 서비스 확장 고려된 설계, 작은 서비스 단위의 독립적 설계, 개발 가능
    - 신규 서비스 개별서비스로 추가 가능, 서비스 통신 복장, 데이터 정합성 확보 어려움 -> 빠른 개발, 효율적이 조직 운영의 장정이 더 크다.
    - 클라우드 서비스의 아키텍쳐로 장점이 더 많다.
   
   
### DevOps
- 개발과 운영의 Seamless 한 연결 및 통합 필요, 자동화된 하나의 프로세스로 통합 필요
-  노하우 공유, 조직 기술역량 고도화

### CI/CD
- 지속적인 통합, 소스 저장소, 빌드 ,테스트 , 실행 등의 일련의 절차를 통해 시간 단축
- 상용서비스 적용 및 배포
    - 도구: git, jenkins, terraform, elastic sear4ch 등등..
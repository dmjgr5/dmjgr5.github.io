---
layout: default
title: MAS
parent: Cloud
# has_children: true
nav_order: 6
---



# MSA
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}




{: .highlight }
단일 서버에서 서비스 하던 것에서 벗어나 가능한 작은 서비스들로 나눈어 클라우드 환경에서 구성하기 위함이다. 
 MicroService  -> Contailer -> 많아지니까 클러스터 구성 -> 이런 환경을 클라우드 환경에서 구현함
 
---

![https://www.s-core.co.kr/ 참조](/assets/images/msa-arch.png) 

    1. Monolith 한 시스템
    2. VMdnldp 가상환경 만들어 서비스
    3. VM 위에 작은  서비스 만듦
        - 많아진다, 관리하기 어렵다. 
        - 쿠버네티스 오픈 소스 제공(많은 서비스를 넣어 놓고 스케일업 할 수 있음)
        - 결국 작은 서비스를 쉽고 빠르게 운용하기 위함


### MSA 정의

##### MicroService 와 Architecture 로 구분해서 이해해보자. 

    MicroService : 서비스를 가능한 작게, 도메인 기준으로, 독립적으로 동작 가능하게 하나는 개념
    Architecture :기술적으로 MicroService 를 어떻게 구현할 것지 

### 주요 고려사항
    1. 게이트웨이 :어느 서비스로 갈지를 정해야 하기 때문에 필요하다.(넷플릭스의 Zuul)
        - 여러 서비스의 주소를 게이트웨이의 단일 주소로 관리(테스트 시 용이)
    2. 서비스 발견 : 어느 서비스로 갈지를 정해야 하기 때문에 필요하다.
    3. 레지스트리 :서비스 정보를 보관(넷플릭스의 Erueka)
        - 많은 서비스의 상태, 주소기록/관리 : 서비스명만 알면 가능하도록 한다. 
    4. 서비스 간 통신 :타 서비스가 필요할 때 서비스끼리 활용할 수 있도록 한다.
        - 다른 서비스한테 바로 알려줘, 또는 넣어놓을테니 필요할 때 처리해
            - CircuitBreaker : 다른 서비스끼리 통신하다 하나의 서비스가 안될 때
    5. 로깅/모니터링 : 다수 서비스에 대한 로그 확인 필요 (오픈소스 예: Fluentd -> ElasticSearch -> Kibana)


### 패턴화
    실질적으로 어떻게 구현하고 무엇으로 개발해야 될지를 다이어그램으로 제공(CNCF)
    
![generateFolder](/assets/images/msa-cncf.jpg)

### 주요 활용기업
    - 넷플릭스 : SpringBoot + SpringCloud library => SpringCloud 로 Maven  Repository(Maven.org ) 검색



### 시연 내용 정리 

>각각의 서비스가 이클립스 상에서 각각의 Project 라고 보면 됨
swagger : 개발된 API 를 테스트할 수 있는 화면
재고 서비스가 죽더라도 다른 서비스들은 살아 있어야 한다. 


```text
서비스:         재고서비스         생산서비스          주문서비스
                          |                           |                          |
화면 :             swagger(1)    swagger(2)      swagger(3)
                          |                          |                          | 
DB:                RabbitMQ        MariaDB          MongoDB

```

#### 호출 방법

    방법1. 다른 서비로 직접 호출하는 방법
        -     1. HTTP REST API 호출 : 일반적으로 간단한 데이터
        -     2 . 다른 서비스 죽었을 때 살아있는 서비스는 문제없이 된다. 단 복구 이후에 수량 안맞으면 맞춰준다.
            - 보상트랜잭션:MSA 구성시 보상트랜잭션 설계가 중요하다.
    
    
    방법2) 간접적으로 다른 서비스의 큐(RabbitMQ) 를 활용한다.
        - 간접 큐 호출 시에도 문제없이 작동하지만 살아있을 때 큐에 적재되었던 것을 그때 처리한다.
     
     
### 쿠버네티스 요약
   
   ```
          POD (서비스마다 하나씩 들어가 있음)
                        |
                     Service
                        | 
  Ingress (외부에서 접속할 수 있음, 쿠버네티스 안에서 네이밍 역할)
   
   
   ```

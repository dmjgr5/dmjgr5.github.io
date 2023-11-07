---
layout: default
title: Spring 기본 개념
parent: Spring
grand_parent: Framework
permalink: /docs/framework/spring-basic/
nav_order: 10
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---





`남궁성의 Spring framework 강좌` 가 개인적으로 가장 잘 설명해주시고 이해하기 쉬웠던 강의라 생각한다. 잊지 않기 위해 정리한다.



> 참조자료 : [https://github.com/castello/spring\_basic](https://github.com/castello/spring_basic)

![](/assets/images/spring1.png)

![](/assets/images/spring2.png)


## Tomcat

- `webapps` 폴더에 `war` 파일을 넣어주면 해당 파일 압축 후에 서비스 된다.

## HTTP 요청과 응답

- QueryString 을 처리하는 방법
    - `@Controller` -> `@RequestMapping` -> `request.getParameter("aaa")`
- HTTP 요청 -> `HttpServeltRequest request` 매개변수 를 받아서 톰캣에서 처리한다.

```bash
[실행결과] http://localhost:8080/ch2/requestInfo?year=2021&month=10&day=1 
request.getCharacterEncoding()=UTF-8
request.getContentLength()=-1
request.getContentType()=null
request.getMethod()=GET
request.getProtocol()=HTTP/1.1
request.getScheme()=http
request.getServerName()=localhost
request.getServerPort()=8080
request.getRequestURI()=http://localhost:8080/ch2/requestInfo
request.getRequestURI()=/ch2/requestInfo
request.getContextPath()=/ch2
request.getServletPath()=/requestInfo
request.getQueryString()=year=2021&month=10&day=1
request.getLocalName()=localhost
request.getLocalPort()=8080
request.getRemoteAddr()=0:0:0:0:0:0:0:1   <--- AWS에 배포(deploy)한 다음에 실행하면, 실제 ip주소를 확인할 수 있음.
request.getRemoteHost()=0:0:0:0:0:0:0:1   <--- AWS에 배포(deploy)한 다음에 실행하면, 실제 ip주소를 확인할 수 있음.
request.getRemotePort()=54855
```

- `HttpServeltResponse` 를 매개변수에 넣어 response 객체를 얻는다.

---

## WAS

`Web Application Server` : 서버에 프로그램을 설치한다. 예) 프로그램 업데이트를 서버에서만 하면 된다.

---

## 설정파일

### server.xml

-   톰캣 설치경로/conf/server.xml : 톰갯 서버 설정파일

### web.xml

-   톰갯 설치경로/conf/web.xml : Tomcat 의 모든 web app 의 공통설정
-   웹앱이름/WEB-INF/web.xml : wep app 의 개별 설정

## Tabbed Postman

-   크롬 확장프로그램에서 제공되는 Rest Client 로 포스트맨 설치 대신 사용이 가능하다.

---

## 저장소

### pageContext : page 별

### request : request 별

### session : 사용자별

### application : context 전체

---

## 서블릿 > JSP > 스프링 > 스프링부트

## @RequestParam 과 @ModelAttribute

## @GetMapping , @PostMapping

## redirect 와 forward

## 쿠키 : 브라우저에 저장

## 세션 : 서버에 저장

-   무조건 서버에서 생성하여 response 헤더에 세션정보를 넣어준다.

---

## 예외처리

-   try-catch 를 사용한다.
-   `@ContrlloerAdvice` 내 `@ExceptionHandler` 에서 처리하도록 한다.
-   `@ResponseStatus` : 응답메시지의 상태코드를 변경할 때 사용한다.

---

## Spring DI

## Java Beans > Servlet & JSP bean > EJB(복잡) > Spring Bean

## SQL & SQL Workbench

## Spring DB 다루기 & Junit Test

## DAO

## AOP 개념과 용어 : 중복코드 분리위함

-   메서드의 시작 또는 끝에 자동으로 코드(advice) 를 실행 중에 추가
-   횡단관심사 : 예) logging
-   `@Before`, `@After`, `@Around`

## Controller > Service > Repository(Dao) > DB

-   위 3개 모두 Component 스캔이 된다.

## @Transactional : 롤백 가능

-   `REQUIRES_NEW` : 현재와 다른 새로운 transaction 생성한다. (default : REQUIRED)

## Redirect : 300번 응답, 2번 요청 2번 응답, 두번쨰는 브라우저가 자동 GET 요청

## Forward : 다른 곳으로 요청
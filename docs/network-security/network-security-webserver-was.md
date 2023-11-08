---
layout: default
title: Web Server & WAS
parent: Network & Security
nav_order: 2
---


# Web Server & WAS
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Static Pages

-   Web Server는 파일 경로 이름을 받아 경로와 일치하는 file contents를 반환합니다.
-   항상 동일한 페이지를 반환합니다.
-   html, css, js, image 파일과 같이 컴퓨터에 저장되어 있는 파일들입니다.


![](/assets/images/webserver1.png)

*[정적 페이지 ]*
{: .text-center }


## Dynamic Pages

-   인자의 내용에 맞게 동적인 Contents를 반환합니다.
-   즉, 웹 서버에 의해서 실행되는 프로그램을 통해서 만들어진 결과물입니다.
-   아래 이미지에서 개발자는 Servlet에 doGet()을 구현합니다.

 
![](/assets/images/webserver2.png)

*[동적 페이지 ]*
{: .text-center }


## Web Server와 WAS의 차이

### Web Server

웹 서버는 클라이언트로부터 HTTP 요청을 받아 HTML 문서나 각종 리소스(Resource)를 전달하는 컴퓨터입니다. 따라서 요청에 따라 아래의 두 가지 기능 중 적절하게 선택하여 수행합니다.

-   정적인 컨텐츠를 제공한다.
-   동적인 컨텐츠 제공을 위한 요청을 전달한다.

Web Server에는 `Apache Server`와 `NginX`가 있는데 각각의 특성은 아래와 같습니다.

-   Apache Server: BSD, Linux 등 Unix계열뿐 아니라 Windows와 같은 기종에서도 운용 가능.
-   NginX: 가벼움과 높은 성능을 목표로 함, 웹 서버, 리퍼스 프록시 및 메일 프록시 기능을 가짐.

> Apache Tomcat이라고 하는 이유
> 
> 웹서버는 Apache, WAS는 Tomcat  
> 2008년에 릴리즈된 Tomcat 5.5 부터 정적 컨텐츠 처리기능 추가  
> \-> 순수 Apache를 사용하는것에 비해 성능 차이 없음  
> \-> Tomcat이 Apache의 기능을 포함하므로 Apache Tomcat이라고함

### Web Application Server, WAS

WAS는 웹 애플리케이션과 서버 환경을 만들어 동작시키는 기능을 제공하는 **소프트웨어 미들웨어 프레임워크** 입니다.
 


![](/assets/images/webserver3.png)

*[Web Application Server, WAS]*
{: .text-center }


위 이미지와 같이 WAS는 `Web Server`와 `Web Container`(JSP, Servlet)으로 이루어져 있습니다. Web Server와의 차이점은 Web Container를 가진다는 점이며 WAS는 HTML 같은 정적인 페이지에서 처리할 수 없는 비즈니스 로직이나 DB 조회 같은 동적인 콘텐츠를 제공합니다.

## Web Server 와 WAS 를 분리하는 이유

그런데 대규모 프로젝트를 보면 아래와 같이 WAS와 Web Server를 분리한 형태를 볼 수 있습니다. 분리를 하면 생기는 장점이 무엇일까요?



![](/assets/images/webserver4.png)

*[Web Server와 WAS 분리]*
{: .text-center }

**1\. 기능을 분리하여 서버 부하 방지 가능**

WAS는 DB 조회 등 페이지를 만들기 위한 다양한 로직을 처리하는데, 단순한 정적 콘텐츠를 WAS에서 제공한다면 다른 작업에 사용하는 리소스들로 인해 지연이 생겨날 수 있다.  
tomcat 5.5 이상부터는 성능이 크게 떨어지지 않는다고 한다.

**2\. 물리적으로 분리하여 보안 강화**

SSL에 대한 암복호화 처리에 Web Server를 사용한다.  
공격에 대해 Web Server를 앞단에 두어 중요한 정보가 담긴 DB나 로직까지(WAS까지) 전파되지 못하게 한다.

**3\. 여러 대의 WAS를 연결이 가능하다.**

Load Balancing(부하의 균등화) 가능 -> 한 서버가 다운되더라도 서비스를 이어갈 수 있다는 장점이 있다.  
fail over(장애 극복), fail back 가능  
대용량 웹 애플리케이션의 경우(여러 개의 서버 사용) Wev Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.



![](/assets/images/webserver5.png)

*[여러대의 WAS 운용]*
{: .text-center }



**4\. 다른 종류의 WAS로 서비스 가능**
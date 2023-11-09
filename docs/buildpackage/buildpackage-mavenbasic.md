---
layout: default
title: Maven 생성 및 실행
parent: Build & Package
nav_order: 10
--- 

# Maven 생성 및 실행
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---




Maven 의 종류와 사용방법에 대해 논한다.

{: .highlight }
> 아래 링크를 참조하여 작성하였습니다. 
> 
> [https://www.youtube.com/watch?v=hDp\_\_2KmjVg&list=PLq8wAnVUcTFWRRi\_JWLArMND\_PnZM6Yja&index=2](https://www.youtube.com/watch?v=hDp__2KmjVg&list=PLq8wAnVUcTFWRRi_JWLArMND_PnZM6Yja&index=2)


## mvn 자바 프로젝트 생성

```bash
D:\99_Maven\maven_project>mvn archetype:generate -DgroupId=com.newlecture -DartifactId=javaprj -DarchetypeArtifactId=maven-archetype-quickstart
```

설치 완료 후 아래 로그가 나온다.

```bash
 package: com.newlecture
 Y: :
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Old (1.x) Archetype: maven-archetype-quickstart:1.0
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: basedir, Value: D:\99_Maven\maven_project
[INFO] Parameter: package, Value: com.newlecture
[INFO] Parameter: groupId, Value: com.newlecture
[INFO] Parameter: artifactId, Value: javaprj
[INFO] Parameter: packageName, Value: com.newlecture
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] project created from Old (1.x) Archetype in dir: D:\99_Maven\maven_project\javaprj
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  01:29 min
[INFO] Finished at: 2023-01-26T21:53:27+09:00
[INFO] ---
```

## 컴파일과 실행하기

### 컴파일 하기

```bash
D:\99_Maven\maven_project\javaprj>mvn compile
[INFO] Scanning for projects...
```

### jar 파일 생성하기

```bash
D:\99_Maven\maven_project\javaprj>mvn package
[INFO] Scanning for projects...
```

### jar 파일 실행하기

```bash
D:\99_Maven\maven_project\javaprj>java -cp target\javaprj-1.0-SNAPSHOT.jar com.newlecture.App
Hello Maven!
```

## Build LifeCycle 과 Phase 들

### Build LifeCycle

![](/assets/images/bpmaven1.png)
 

단계별로 실행을 담당하는 플러그인들로 이루어져 있다.
 
![](/assets/images/bpmaven2.png)

```bash
D:\99_Maven\maven_project\javaprj>mvn help:describe -Dcmd=compile
.. 중략
It is a part of the lifecycle for the POM packaging 'jar'. This lifecycle includes the following phases:
* validate: Not defined
* initialize: Not defined
* generate-sources: Not defined
* process-sources: Not defined
* generate-resources: Not defined
* process-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:resources
* compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:compile
* process-classes: Not defined
* generate-test-sources: Not defined
* process-test-sources: Not defined
* generate-test-resources: Not defined
* process-test-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:testResources
* test-compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:testCompile
* process-test-classes: Not defined
* test: org.apache.maven.plugins:maven-surefire-plugin:2.12.4:test
* prepare-package: Not defined
* package: org.apache.maven.plugins:maven-jar-plugin:2.4:jar
* pre-integration-test: Not defined
* integration-test: Not defined
* post-integration-test: Not defined
* verify: Not defined
* install: org.apache.maven.plugins:maven-install-plugin:2.4:install
* deploy: org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  28.680 s
[INFO] Finished at: 2023-01-26T22:07:31+09:00
[INFO] ------------------------------------------------------------------------

D:\99_Maven\maven_project\javaprj>
```

## 메이븐 프로젝트 이클립스에서 로드하기

Import > Maven > Exising Maven Projects 선택한다.

이후 pom.xml 이 있는 경로를 지정한다.

ctrl + F11 을 클릭하여 실행을 확인한다.

### 컴파일 플러그인으로 JDK 버전 변경하기

`pom.xml` 에 아래 추가하여 프로젝트 폴더 오른쪽 버튼 maven > update 한다.

```yaml
  <properties>
      <maven.compiler.source>1.8</maven.compiler.source>
      <maven.complier.target>1.8</maven.complier.target>
  </properties>
```

## 웹 프로젝트로 변경하기

### 서블릿/JSP 라이브러리 설정하기

[https://www.youtube.com/watch?v=vlIGi0hG\_n4](https://www.youtube.com/watch?v=vlIGi0hG_n4)

`https://mvnrepository.com/` 사이트에 들어가서 해당되는 lib 를 찾아 아래 maven 탭 내용을 복사하여 `pom.xml` 에 붙여넣기 한 후 저장한다.
 
![](/assets/images/bpmaven3.png)

그 이후에는 dependencies 에 포함되어 있는 것을 확인할 수 있다.

![](/assets/images/bpmaven4.png)
 
### 라이브러리 인덱싱 검색

또 다른 라이브러리 추가 방법으로는 `pom.xml` 파일을 열었을 때 하단의 `Dependencies` 탭에서 `Add` 버튼을 클릭하여 추가할 수 있다. 이때 검색을 위해서는 사전에 `Maven Repository > Global Repositories` 에 인덱싱이 되어있어야 한다. (3시간 소요)

![](/assets/images/bpmaven5.png)
 
### 내가 만든 라이브러리 설치하기

[https://www.youtube.com/watch?v=2fyQm\_gfnHk&list=PLq8wAnVUcTFWRRi\_JWLArMND\_PnZM6Yja&index=12](https://www.youtube.com/watch?v=2fyQm_gfnHk&list=PLq8wAnVUcTFWRRi_JWLArMND_PnZM6Yja&index=12)
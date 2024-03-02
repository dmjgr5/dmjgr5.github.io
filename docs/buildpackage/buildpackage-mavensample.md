---
layout: default
title: Maven 실습하기
parent: Build & Package
nav_order: 11
--- 

# Maven 실습하기


{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
{:toc}

---

{: .highlight }
> https://wikidocs.net/book/1910 를 참조하여 작성하였다.


## Maven Hello World 프로젝트 시작

Visual Studio Code, Eclipse등 어떠한 에디터도 상관 없음.
빈 프로젝트로 시작

### 특정 프로젝트 경로 최상위에 pom.xml을 생성

- <project> : 최상위 태그
- <groupId> : 프로젝트의 그룹명, 일반적으로 다른 컴포넌트와 라이브러리와 차별될 수 있는 유니크한 명칭을 가짐
  관례적으로 회사 도메인명(google.com, naver.com)을 거꾸로한 명칭을 사용함
  중앙의 공용 Maven Repository에서 수많은 라이브러리를 서비스하면서 unique한 groupId를 가지게 하기 위한 관례가 아닐까 추측됨

- <artifactId> : 해당 프로젝트 명칭(컴포넌트 명칭), groupId 범위 내에서 유일해야함

- <modelVersion> : pom.xml을 이루고 있는 maven xml문서 형식의 버전이다. 현재는 무조건 4.0.0 이다. 5.0.0 은 draft 상태이며 정식 발표되지 않았다.
  https://cwiki.apache.org/confluence/display/MAVEN/POM+Model+Version+5.0.0

- <version> : 해당 artifact(컴포넌트)의 version, 뒤쪽 SNAPSHOT은 아직 개발 중임을 의미함

- <packaging> : 어떤 파일 형식으로 패키징할 것인가를 정의, jar, war, exe 등이 올 수 있음

```sh
<project>
    <groupId>com.blidkaga</groupId>
    <artifactId>HelloWorld</artifactId>
    <modelVersion>4.0.0</modelVersion>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
</project>
```

### 프로젝트에 src/main/java 디렉토리를 생성하고 HelloWorld.java 파일을 생성 후 아래 코드 입력

```java
public class HelloWorld{
    public static void main(String args[]){
        System.out.println("Maven Hello World!");
    }
}
```


### compile 버전 정보 입력

위쪽 1번 pom정보가 기본적으로 작성되어야할 항목이지만, 현재 시점에서는 컴파일시 아래와 같은 에러가 발생함
compile을 상위 Java버전으로 컴파일을 수행할 추가 정보를 입력함

```sh
[ERROR] Source option 5 is no longer supported. Use 6 or later.
[ERROR] Target option 1.5 is no longer supported. Use 1.6 or later.
<project>
    ... 생략
    <build>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.7.0</version>
            <configuration>
            <source>소스코드 Java 버전(ex 1.8)</source>
            <target>compile시 결과물 Java 버전(ex 1.8)</target>
            </configuration>
        </plugin>
        </plugins>
    </build>
</project>
```

### 빌드 수행

빌드를 하기전 빌드 결과 위치를 비우는 명령어, 콘솔 명령창에서 수행

```sh
$ mvn clean
```

compile 수행 명령어
compile 후 target 디렉토리가 생성되며 컴파일된 결과물이 만들어짐
target/classes/HelloWorld.class 파일 확인 가능

```sh
$ mvn compile
```

### 실행

아래의 명령어로 실행하면 Maven Hello World! 가 정상적으로 찍힌다.
-cp : 클래스 패스 경로

```sh
$ java -cp target/classes HelloWorld
Maven Hello World!
```

### 패키징

java 패키징 명령어
target/HelloWorld-1.0-SNAPSHOT.jar 파일이 생성된다.

```sh
$ mvn package
```

### 패키징 된 파일 실행

```sh
$ java -jar target/HelloWorld-1.0-SNAPSHOT.jar HelloWorld
```

## Pom.xml 기본 구조

### 프로젝트 정보

- modelVersion
- groupId
- artifactId
- version
- packaging

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.blidkaga.group</groupId>
    <artifactId>group-test</artifactId>
    <name>test</name>
    <packaging>war</packaging>
    <version>1.0.0-SNAPSHOT</version>
</project>
```

### properties

maven 내부에서 반복적으로 사용될 상수 값을 정의할 때 사용

```xml
<properties>
    <java-version>1.6</java-version>
    <org.springframework-version>3.1.1.RELEASE</org.springframework-version>
    <org.aspectj-version>1.6.10</org.aspectj-version>
    <org.slf4j-version>1.6.6</org.slf4j-version>
</properties>
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${org.springframework-version}</version>
        <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
        </exclusions>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${org.springframework-version}</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${org.springframework-version}</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

### Dependencies(의존성 라이브러리 정보)

project 태그 바로 하위
의존성 라이브러리 정보를 포함함
최소 groupId, artifactId, version 정보가 필요함
dependencies 색션 아래 새부 `dependency'가 기술됨

```xml
    ... 생략
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${org.springframework-version}</version>
            <exclusions>
                <exclusion>
                    <groupId>commons-logging</groupId>
                    <artifactId>commons-logging</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${org.springframework-version}</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```


### 빌드 정보

project 태그 바로 하위
plugins : 빌드에서 사용할 플러그인
디렉토리 구조

```xml
    ... 생략
    <build>
        <sourceDirectory>src/main/java</sourceDirectory>
        <testSourceDirectory>src/test/java</testSourceDirectory>
        <outputDirectory>${project.basedir}/target/classes</outputDirectory>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>resources-${deploy.phase}/</directory>
                <filtering>true</filtering>
            </resource>
        </resources>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.5.1</version>
                <configuration>
                    <source>1.6</source>
                    <target>1.6</target>
                    <compilerArgument>-Xlint:all</compilerArgument>
                    <showWarnings>true</showWarnings>
                    <showDeprecation>true</showDeprecation>
                </configuration>
            </plugin>
            </plugin>
        </plugins>
    </build>
```

### Repository

의존성을 다운로드 받을 위치의 repository
기술되지 않을 시 기본적인 위치
http://repo.maven.apache.org/maven2
다수의 <repository> 기술 가능
회사 내부의 repository를 기술 하기도 한다.
nexus
artifactory를 이용

```xml
    ...생략   
    <repositories>
        <repository>
            <id>spring-snapshot</id>
            <name>Spring Snapshot Repository</name>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>false</enabled>
            </releases>
        </repository>
    </repositories>
```

### Plugin Repository

maven plugin을 다운로드 받을수 있는 저장소 위치를 기술

```xml
<pluginRepositories>
    <pluginRepository>
        <id>acme corp</id>
        <name>Acme Internal Corporate Repository</name>
        <url>http://acmecorp.com/plugins</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </pluginRepository>
</pluginRepositories>
```

### 배포

deploy goal을 실행했을 시 배포되는 위치를 기술함
url 항목에는 일반적으로 회사 내부에 설치한 repository url이 기술된다.(nexus, artifactory등등으로 설치된..)

```xml
<distributionManagement>
    <repository>
        <id>releases-repo</id>
        <name>Releases Repository</name>
        <url>일반적인 회사 내부 repository url</url>
    </repository>
    <snapshotRepository>
        <id>snapshots-repo</id>
        <name>Snapshots Repository</name>
        <url>일반적인 회사 내부 repository url</url>
    </snapshotRepository>
</distributionManagement>
```

## Goal

- Maven이 행할수 있는 여러가지 동작을 수행하는 명령을 Goal이라고함
- 실행 방식, 체인 형태로 실행가능

```
$ mvn [goal명령어] [goal명령어] [goal명령어] 
```
 

### clean 

컴파일 결과물인 target 디렉토리 삭제

### compile 
모든 소스코드 컴파일, 리소스파일을 target/classes 디렉토리에 복사

### package
compile 수행 후, 테스트 수행, <packaging> 정보에 따라 패키징 수행

### install
package 수행 후, local repo에 install 수행

### deploy
install 수행 후, 배포 수행, 여기서 배포는 웹서버에 배포가 아니다. 회사 repo에 배포다.

아래와 같이 distributionManagement 항목이 기술되어야 한다.

```xml
...생략
    <distributionManagement>
        <repository>
            <id>releases-repo</id>
            <name>Releases Repository</name>
            <url>회사repository주소(넥서스)</url>
        </repository>
        <snapshotRepository>
            <id>snapshots-repo</id>
            <name>Snapshots Repository</name>
            <url>회사repository주소(넥서스)</url>
        </snapshotRepository>
    </distributionManagement>
```


## Repository(저장소)

Maven은 Repository라는 개념이 있으며, Local Repository, Repository, Plugin Repository 3가지로 구분된다.

### 1. Local Repository(로컬 저장소)

Maven은 dependency 및 packaging한 프로젝트 결과물을 local에 특정 위치에 저장

```
Linux & Mac : <홈디렉토리>/.m2
Windows : C:\Users\<유저이름>/.m2
```

.m2 디렉토리 하위에 dependency 및 packaging한 결과물들을 groupId, artifactId, versions값으로 하위 디렉토리구조가 생성이 되며 저장된다. 6-1

여러 프로젝트에서 중복된 라이브러리 다운로드를 피하기위한 목적을 가진다.

### 2. Repository

- 의존성을 다운로드 받을 위치의 repository
- 기술되지 않을 시 기본적인 위치 http://repo.maven.apache.org/maven2
- 다수의 <repository> 기술 가능
- 회사 내부의 repository를 기술 하기도 한다.
  - nexus
  - artifactory를 이용

```xml
    ...생략   
    <repositories>
        <repository>
            <id>spring-snapshot</id>
            <name>Spring Snapshot Repository</name>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
            <releases>
                <enabled>false</enabled>
            </releases>
        </repository>
    </repositories>
```

### 3. Plugin Repository

maven plugin을 다운로드 받을수 있는 저장소 위치를 기술

- 다수 <pluginRepository> 기술 가능

```xml
<pluginRepositories>
    <pluginRepository>
        <id>acme corp</id>
        <name>Acme Internal Corporate Repository</name>
        <url>http://acmecorp.com/plugins</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </pluginRepository>
</pluginRepositories>

```
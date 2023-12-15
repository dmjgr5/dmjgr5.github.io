---
layout: default
title: Credentials
parent: Jenkins
grand_parent: CI/CD Tools
permalink: /docs/cicdtools/credentials
nav_order: 3
---

# Credentials
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .note }
> 젠킨스에서의 Credential 은 자주 쓰이면서도 중요한 요소 중 하나이다. 일반적으로 빌드를 위한 소스를 Clone 시에 github 와 같은 SCM 에서 가져오기 위한 인증 정보를 보관하는 곳이라고 보면 된다.
 전역적으로 설정하는 것 이외에 특정 잡을 위해 폴더 내에서 관리되는 Credential 에 대해 정리해 본다.



----

## Folder 생성하기

New item 에서 Folder 를 선택하여 폴더명 입력 후 저장한다. 별도 설정 정보는 없어도 된다.

## Job 생성하기

생성한 폴더 내에서 빌드될 Job 을 생성한다.

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                script { 

    				sh 'pwd'
                    sh 'ls -al'
                    
                    checkout(
                        [$class: 'GitSCM', 
                        branches: [[name: 'main' ]], 
                        doGenerateSubmoduleConfigurations: false, 
                        //extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'Sub1']], 
                        extensions: [[$class: 'CleanCheckout']], 
                        submoduleCfg: [], 
                        userRemoteConfigs: [
                            [credentialsId: 'folder-credential', 
                            url: 'https://github.com/dmjgr5/spring-vue-starter.git' ]] 
                        ]
                    )
						
					sh 'pwd'
                    sh 'ls -al'
                    
                }
            }
        }
    }
}

```

생성 후 Job 을 빌드하면 github private repository 의 경우 아래와 같이 인증 에러를 만난다.

```sh
fatal: Authentication failed for 'https://github.com/dmjgr5/spring-vue-starter.git/'
```

## Credential 생성하기

Folder > Credential 에서 직접 생성할 수도 있지만, 본 장에서는 ColudBee 플러그인을 사용하여 Rest API 로 생성해본다.
 

{: .note}
> CloudBees Credentials Plugin 을 설치한 후 아래 링크를 참조하여 진행한다.
> 
> [https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-controllers/how-to-manage-credentials-via-the-rest-api](https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-controllers/how-to-manage-credentials-via-the-rest-api)


```sh
POST http://localhost:9090/job/myfolder/credentials/store/folder/domain/_/createCredentials

<com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
  <scope>GLOBAL</scope>
  <id>folder-credential</id>
  <description>This is an example from REST API</description>
  <username>dmjgr5</username>
  <password>ghp_tokentokentokentokentokentokentokento</password>
</com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
```

아래와 같이 폴더 하위에 Credential 이 생성된 것을 확인할 수 있다.

![example](/assets/images/jenkins1.png)

## Credential 인증 확인하기

빌드하면 아래와 같이 정상적으로 checkout 이 완료됨을 확인할 수 있다.

```sh
Started by user user1
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/myfolder/myjob
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ pwd
/var/lib/jenkins/workspace/myfolder/myjob
[Pipeline] sh
+ ls -al
total 12
drwxr-xr-x 3 jenkins jenkins 4096 12월 15 23:17 .
drwxr-xr-x 6 jenkins jenkins 4096 12월 15 23:17 ..
drwxr-xr-x 8 jenkins jenkins 4096 12월 15 23:41 .git
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
using credential folder-credential
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/myfolder/myjob/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/dmjgr5/spring-vue-starter.git # timeout=10
Fetching upstream changes from https://github.com/dmjgr5/spring-vue-starter.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials This is an example from REST API
 > git fetch --tags --force --progress -- https://github.com/dmjgr5/spring-vue-starter.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse origin/main^{commit} # timeout=10
Checking out Revision 014eeb6c3ff531dfb32e999b8b050abf1b187983 (origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 014eeb6c3ff531dfb32e999b8b050abf1b187983 # timeout=10
Commit message: "dsf"
First time build. Skipping changelog.
Cleaning workspace
 > git rev-parse --verify HEAD # timeout=10
Resetting working tree
 > git reset --hard # timeout=10
 > git clean -fdx # timeout=10
[Pipeline] sh
+ pwd
/var/lib/jenkins/workspace/myfolder/myjob
[Pipeline] sh
+ ls -al
total 24
drwxr-xr-x 4 jenkins jenkins 4096 12월 15 23:42 .
drwxr-xr-x 6 jenkins jenkins 4096 12월 15 23:17 ..
drwxr-xr-x 8 jenkins jenkins 4096 12월 15 23:42 .git
-rw-r--r-- 1 jenkins jenkins  442 12월 15 23:42 Jenkinsfile
-rw-r--r-- 1 jenkins jenkins   48 12월 15 23:42 README.md
drwxr-xr-x 5 jenkins jenkins 4096 12월 15 23:42 springVueStarter
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

## Credential 업데이트 

편의를 위해 username 과 password 를 각각 업데이트 한 후 변경 사항을 확인해보자.

```sh
POST http://localhost:9090/job/myfolder/credentials/store/folder/domain/_/credential/folder-credential/config.xml

<com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
  <scope>GLOBAL</scope>
  <id>folder-credential</id>
  <description>This is an example from REST API</description>
  <username>xxxdmjgr5</username>
  <password>ghp_tokentokentokentokentokentokentokento</password>
</com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
```

아래와 같이 username 이 변경됨을 확인할 수 있다.



![example](/assets/images/jenkins2.png)


이번엔 password 를 변경해 보자.



```sh
POST http://localhost:9090/job/myfolder/credentials/store/folder/domain/_/credential/folder-credential/config.xml

<com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
  <scope>GLOBAL</scope>
  <id>folder-credential</id>
  <description>This is an example from REST API</description>
  <username>dmjgr5</username>
  <password>xxxghp_tokentokentokentokentokentokentokento</password>
</com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
```

예상대로 인증 에러가 발생했다.

```sh
fatal: Authentication failed for 'https://github.com/dmjgr5/spring-vue-starter.git/'
```

이번엔 password 가 정상인데, username 이 잘못되면 어떻게 될까.

```sh
POST http://localhost:9090/job/myfolder/credentials/store/folder/domain/_/credential/folder-credential/config.xml

<com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
  <scope>GLOBAL</scope>
  <id>folder-credential</id>
  <description>This is an example from REST API</description>
  <username>xxxdmjgr5</username>
  <password>ghp_tokentokentokentokentokentokentokento</password>
</com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>
```
{: .note}
> 예상과는 다르게 checkout 이 정상적으로 수행된다. 정리하면 username, password 로 Credentials 를 관리하는데 github 에서의 checkout 은 username 과는 관계없이 password, 즉 토큰정보만을 이용해서 인증이 수행됨을 판단할 수 있다.

## Credentials 조회하기

Credential Id 로 조회하면 해당 정보 확인이 가능하다. 만약 해당 credential 이 없다면, 404Not Found 에러가 발생한다.


```sh
GET http://localhost:9090/job/myfolder/credentials/store/folder/domain/_/credential/folder-credential/config.xml

---

<com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl plugin="credentials@1293.vff276f713473">
    <scope>GLOBAL</scope>
    <id>folder-credential</id>
    <description>This is an example from REST API</description>
    <username>xxxdmjgr5</username>
    <password>
        <secret-redacted/>
    </password>
</com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl>

```

## Credentials 삭제하기

아래 URL 로 정상 삭제가 가능하다.

```sh
DELETE http://localhost:9090/job/myfolder/credentials/store/folder/domain/_/credential/folder-credential/config.xml
```

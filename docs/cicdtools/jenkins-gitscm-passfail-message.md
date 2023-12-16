---
layout: default
title: Jenkins Pipeline 결과 메시지 가져오기
parent: Jenkins
grand_parent: CI/CD Tools
permalink: /docs/cicdtools/passfail-message
nav_order: 7
---

# Jenkins Pipeline 결과 메시지 가져오기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .note }
> 빌드 결과는 post action 내 success, failure 등을 통해 확인할 수 있다. 본 장에서는 관련한 로그를 확인해보고자 한다.




----

## 테스트 스크립트

### Dockerfile

```sh
FROM openjdk:8-jdk-alpine

RUN apk --no-cache add tzdata && cp /usr/share/zoneinfo/Asia/Seoul /etc/localtime
  
COPY ./entrypoint/entrypoint.sh run.sh
RUN chmod 774 run.sh

ENV PROFILE=local
 
ENTRYPOINT ["/bin/echo", "Hello world"] 
```

### Jenkins Pipeline

```groovy
pipeline {
	agent any
    options {
        // This is required if you want to clean before build
        skipDefaultCheckout(true)
    }
	stages {
		stage('Checkout') {
			steps {
			    // Clean before build
                cleanWs()
                
				script {
				
						echo '================  before checkout'
						sh 'tree'
						sh 'ls -al'
						
					  // Checkout code from GitHub 
						checkout(
							[$class: 'GitSCM', 
							branches: [[name: 'main' ]], 
							doGenerateSubmoduleConfigurations: false, 
							//extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'TestDir/Sub1']], 
							extensions: [[$class: 'CleanCheckout']], 
							submoduleCfg: [], 
							userRemoteConfigs: [
								[credentialsId: 'my-github-access-token', url: 'https://github.com/dmjgr5/sample-docker.git' ]]
							]
						)
						
						echo '================  after checkout'
						sh 'tree'
						sh 'ls -al'
				}		
			}
		}

		stage('Build Docker Image') {
			steps {
				// Build Docker image
				script { 
					
					
					sh 'pwd'
					sh 'tree'
					sh 'ls -al'
				 
				 
					echo '================  before build'
					
					sh "docker build  -f dockerfile/Dockerfile-test -t dmjgr5/sample-docker-image-name:v123  ."
					
					echo '================  after build'
					
					sh 'pwd'
					sh 'tree'
					sh 'ls -al'
				}
			}
		}
	}
	
    post {
        always {
            echo 'One way or another, I have finished' 
        }
        success {
            echo 'I succeeded!'
        }
        failure {
            echo 'I failed '
       
        }
    }
}
```

## Success 시 빌드 로그

- 빌드 정보 : http://localhost:9090/job/docker-build-sample/40/

```sh
Started by user user1
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/docker-build-sample
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] cleanWs
[WS-CLEANUP] Deleting project workspace...
[WS-CLEANUP] Deferred wipeout is used...
[WS-CLEANUP] done
[Pipeline] script
[Pipeline] {
[Pipeline] echo
================  before checkout
[Pipeline] sh
+ tree
.

0 directories, 0 files
[Pipeline] sh
+ ls -al
total 8
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:32 .
drwxr-xr-x 11 jenkins jenkins 4096 12월 16 16:32 ..
[Pipeline] checkout
The recommended git tool is: NONE
using credential my-github-access-token
Cloning the remote Git repository
Cloning repository https://github.com/dmjgr5/sample-docker.git
 > git init /var/lib/jenkins/workspace/docker-build-sample # timeout=10
Fetching upstream changes from https://github.com/dmjgr5/sample-docker.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- https://github.com/dmjgr5/sample-docker.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url https://github.com/dmjgr5/sample-docker.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse origin/main^{commit} # timeout=10
Checking out Revision 9ecc520ba22e9baa1d705b394296d65781652ec1 (origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 9ecc520ba22e9baa1d705b394296d65781652ec1 # timeout=10
Commit message: "Update Dockerfile-test"
 > git rev-list --no-walk 725cd5e624d9c6db06a6e8555a4de0ea37f56534 # timeout=10
Cleaning workspace
 > git rev-parse --verify HEAD # timeout=10
Resetting working tree
 > git reset --hard # timeout=10
 > git clean -fdx # timeout=10
[Pipeline] echo
================  after checkout
[Pipeline] sh
+ tree
.
├── dockerfile
│   └── Dockerfile-test
├── entrypoint
│   └── entrypoint.sh
└── README.md

2 directories, 3 files
[Pipeline] sh
+ ls -al
total 24
drwxr-xr-x  5 jenkins jenkins 4096 12월 16 16:32 .
drwxr-xr-x 11 jenkins jenkins 4096 12월 16 16:32 ..
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:32 dockerfile
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:32 entrypoint
drwxr-xr-x  8 jenkins jenkins 4096 12월 16 16:32 .git
-rw-r--r--  1 jenkins jenkins   15 12월 16 16:32 README.md
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build Docker Image)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ pwd
/var/lib/jenkins/workspace/docker-build-sample
[Pipeline] sh
+ tree
.
├── dockerfile
│   └── Dockerfile-test
├── entrypoint
│   └── entrypoint.sh
└── README.md

2 directories, 3 files
[Pipeline] sh
+ ls -al
total 24
drwxr-xr-x  5 jenkins jenkins 4096 12월 16 16:32 .
drwxr-xr-x 11 jenkins jenkins 4096 12월 16 16:32 ..
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:32 dockerfile
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:32 entrypoint
drwxr-xr-x  8 jenkins jenkins 4096 12월 16 16:32 .git
-rw-r--r--  1 jenkins jenkins   15 12월 16 16:32 README.md
[Pipeline] echo
================  before build
[Pipeline] sh
+ docker build -f dockerfile/Dockerfile-test -t dmjgr5/sample-docker-image-name:v123 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  160.8kB

Step 1/6 : FROM openjdk:8-jdk-alpine
 ---> a3562aa0b991
Step 2/6 : RUN apk --no-cache add tzdata && cp /usr/share/zoneinfo/Asia/Seoul /etc/localtime
 ---> Using cache
 ---> 071bb46ca0a3
Step 3/6 : COPY ./entrypoint/entrypoint.sh run.sh
 ---> Using cache
 ---> 28751e88c1bd
Step 4/6 : RUN chmod 774 run.sh
 ---> Using cache
 ---> b22bfea7a287
Step 5/6 : ENV PROFILE=local
 ---> Using cache
 ---> e3531dce33bd
Step 6/6 : ENTRYPOINT ["/bin/echo", "Hello world"]
 ---> Using cache
 ---> 678eeed1ee09
Successfully built 678eeed1ee09
Successfully tagged dmjgr5/sample-docker-image-name:v123
[Pipeline] echo
================  after build
[Pipeline] sh
+ pwd
/var/lib/jenkins/workspace/docker-build-sample
[Pipeline] sh
+ tree
.
├── dockerfile
│   └── Dockerfile-test
├── entrypoint
│   └── entrypoint.sh
└── README.md

2 directories, 3 files
[Pipeline] sh
+ ls -al
total 24
drwxr-xr-x  5 jenkins jenkins 4096 12월 16 16:32 .
drwxr-xr-x 11 jenkins jenkins 4096 12월 16 16:32 ..
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:32 dockerfile
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:32 entrypoint
drwxr-xr-x  8 jenkins jenkins 4096 12월 16 16:32 .git
-rw-r--r--  1 jenkins jenkins   15 12월 16 16:32 README.md
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] echo
One way or another, I have finished
[Pipeline] echo
I succeeded!
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

### 분석 내용

docker build 수행 전에는 항상 `Sending build context to Docker daemon` 문구로 시작한다.
build 성공 후에는 항상 `Successfully tagged ` 로 종료된다.


```sh

Sending build context to Docker daemon  160.8kB

Step 1/6 : FROM openjdk:8-jdk-alpine
 ---> a3562aa0b991
Step 2/6 : RUN apk --no-cache add tzdata && cp /usr/share/zoneinfo/Asia/Seoul /etc/localtime
 ---> Using cache
 ---> 071bb46ca0a3
Step 3/6 : COPY ./entrypoint/entrypoint.sh run.sh
 ---> Using cache
 ---> 28751e88c1bd
Step 4/6 : RUN chmod 774 run.sh
 ---> Using cache
 ---> b22bfea7a287
Step 5/6 : ENV PROFILE=local
 ---> Using cache
 ---> e3531dce33bd
Step 6/6 : ENTRYPOINT ["/bin/echo", "Hello world"]
 ---> Using cache
 ---> 678eeed1ee09
Successfully built 678eeed1ee09
Successfully tagged dmjgr5/sample-docker-image-name:v123
[Pipeline] echo
================  after build
```

## Failure 시 빌드 로그 

http://localhost:9090/job/docker-build-sample/39

```sh
Started by user user1
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/docker-build-sample
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] cleanWs
[WS-CLEANUP] Deleting project workspace...
[WS-CLEANUP] Deferred wipeout is used...
[WS-CLEANUP] done
[Pipeline] script
[Pipeline] {
[Pipeline] echo
================  before checkout
[Pipeline] sh
+ tree
.

0 directories, 0 files
[Pipeline] sh
+ ls -al
total 8
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:31 .
drwxr-xr-x 11 jenkins jenkins 4096 12월 16 16:31 ..
[Pipeline] checkout
The recommended git tool is: NONE
using credential my-github-access-token
Cloning the remote Git repository
Cloning repository https://github.com/dmjgr5/sample-docker.git
 > git init /var/lib/jenkins/workspace/docker-build-sample # timeout=10
Fetching upstream changes from https://github.com/dmjgr5/sample-docker.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- https://github.com/dmjgr5/sample-docker.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url https://github.com/dmjgr5/sample-docker.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse origin/main^{commit} # timeout=10
Checking out Revision 725cd5e624d9c6db06a6e8555a4de0ea37f56534 (origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 725cd5e624d9c6db06a6e8555a4de0ea37f56534 # timeout=10
Commit message: "Update Dockerfile-test"
 > git rev-list --no-walk 725cd5e624d9c6db06a6e8555a4de0ea37f56534 # timeout=10
Cleaning workspace
 > git rev-parse --verify HEAD # timeout=10
Resetting working tree
 > git reset --hard # timeout=10
 > git clean -fdx # timeout=10
[Pipeline] echo
================  after checkout
[Pipeline] sh
+ tree
.
├── dockerfile
│   └── Dockerfile-test
├── entrypoint
│   └── entrypoint.sh
└── README.md

2 directories, 3 files
[Pipeline] sh
+ ls -al
total 24
drwxr-xr-x  5 jenkins jenkins 4096 12월 16 16:31 .
drwxr-xr-x 11 jenkins jenkins 4096 12월 16 16:31 ..
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:31 dockerfile
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:31 entrypoint
drwxr-xr-x  8 jenkins jenkins 4096 12월 16 16:31 .git
-rw-r--r--  1 jenkins jenkins   15 12월 16 16:31 README.md
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build Docker Image)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ pwd
/var/lib/jenkins/workspace/docker-build-sample
[Pipeline] sh
+ tree
.
├── dockerfile
│   └── Dockerfile-test
├── entrypoint
│   └── entrypoint.sh
└── README.md

2 directories, 3 files
[Pipeline] sh
+ ls -al
total 24
drwxr-xr-x  5 jenkins jenkins 4096 12월 16 16:31 .
drwxr-xr-x 11 jenkins jenkins 4096 12월 16 16:31 ..
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:31 dockerfile
drwxr-xr-x  2 jenkins jenkins 4096 12월 16 16:31 entrypoint
drwxr-xr-x  8 jenkins jenkins 4096 12월 16 16:31 .git
-rw-r--r--  1 jenkins jenkins   15 12월 16 16:31 README.md
[Pipeline] echo
================  before build
[Pipeline] sh
+ docker build -f dockerfile/Dockerfile-test -t dmjgr5/sample-docker-image-name:v123 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  158.7kB

Step 1/6 : FROM openjdk:8-jdk-alpine
 ---> a3562aa0b991
Step 2/6 : RUN apk --no-cache add tzdata && cp /usr/share/zoneinfo/Asia/Seoul /etc/localtime
 ---> Using cache
 ---> 071bb46ca0a3
Step 3/6 : COPY ./entrypointxxx/entrypoint.sh run.sh
COPY failed: file not found in build context or excluded by .dockerignore: stat entrypointxxx/entrypoint.sh: file does not exist
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] echo
One way or another, I have finished
[Pipeline] echo
I failed 
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
ERROR: script returned exit code 1
Finished: FAILURE
```

### 분석 내용


docker build 수행 전에는 항상 `Sending build context to Docker daemon` 문구로 시작한다.
build 실패 후에는 이후의 스크립트를 스킵한 후 failure 로 이동한다.  

```sh
Sending build context to Docker daemon  158.7kB

Step 1/6 : FROM openjdk:8-jdk-alpine
 ---> a3562aa0b991
Step 2/6 : RUN apk --no-cache add tzdata && cp /usr/share/zoneinfo/Asia/Seoul /etc/localtime
 ---> Using cache
 ---> 071bb46ca0a3
Step 3/6 : COPY ./entrypointxxx/entrypoint.sh run.sh
COPY failed: file not found in build context or excluded by .dockerignore: stat entrypointxxx/entrypoint.sh: file does not exist
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] echo
One way or another, I have finished
```

## 결과 로그 가져오기 

콘솔 정보를 가져오기 위해 아래 URL 을 사용한다.

`curl "${JENKINS_URL}/job/${JOB_NAME}/${BUILD_NUMBER}/consoleText"`

예를 들어 위 success 케이스의 경우 아래 URL 호출을 하여 로그 정보를 가져온다.

`http://localhost:9090/job/docker-build-sample/40/consoleText`



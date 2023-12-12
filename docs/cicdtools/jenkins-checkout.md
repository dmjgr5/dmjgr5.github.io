---
layout: default
title: Credential 변경을 통한 GitScm Clone 하기
parent: Jenkins
grand_parent: CI/CD Tools
permalink: /docs/cicdtools/jenkins-checkout/
nav_order: 2
---

# Credential 변경을 통한 GitScm Clone 하기
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .note }
> Github 의 사용자 계정으로 git clone 을 위해 credential 변경과 pipeline script 를 작성하고 실제 fetch 가 되는지 확인해 본다.



----

## 사용할 Credential 생성하기

`Dashboard > Manage Jenkins > Credentials` 경로에서 사용자 계정을 적용할 Credential 을 사전에 생성한다. 임의의 ID 값(예 : credential-test) 을 정의 후 Save 한다.


## Jenkins Script 작성하기

### 입력받을 parameters 선언

### env variables 정의

### withCredentials 과 checkout 사용하기

```groovy
pipeline {
    agent any
    
    parameters {
        string(name: 'myusername', defaultValue: '', description: 'myusername')
        string(name: 'mypassword', defaultValue: '', description: 'mypassword')
        string(name: 'mybranch', defaultValue: '', description: 'mybranch')
    }
    
    stages {
        stage('Checkout') {

            environment { 
                GIT_BRANCH = "${params.mybranch}"
                GIT_USERNAME = "${params.myusername}"
                GIT_PASSWORD = "${params.mypassword}"
            }

            steps {
                echo("==================== env.GIT_PASSWORD = ${env.GIT_PASSWORD} ====================")
                echo("==================== env.GIT_USERNAME = ${env.GIT_USERNAME} ====================")
                echo("==================== env.GIT_BRANCH = ${env.GIT_BRANCH} ====================")

                
                script {
  
						 
                    withCredentials([usernamePassword(
					                credentialsId: 'credential-test', 
					                passwordVariable: env.GIT_PASSWORD, 
                                    usernameVariable: env.GIT_USERNAME)]) 
                                    {

                                        checkout(
                                            [$class: 'GitSCM', 
                                            branches: [[name: env.GIT_BRANCH ]], 
                                            doGenerateSubmoduleConfigurations: false, 

                                            //make custom directory
                                            //extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'TestDir/Sub1']], 
                                            
                                            extensions: [[$class: 'CleanCheckout']], 
                                            submoduleCfg: [], 
                                            userRemoteConfigs: [
                                                [credentialsId: 'credential-test', 
                                                url: 'https://github.com/gituser/CreateRepoTest.git' ]] 
                                            ]
                                        )

                    }
                
						
					sh 'pwd'
                    sh 'ls -al'
                    
                }
            }
        }
    }
}

```


## Build 요청하기

```sh
POST http://localhost:9090/job/gitcheckout/buildWithParameters?myusername=gituser&mypassword=gittokenvalue&mybranch=main
```

## 결과 로그 확인하기

```sh
Started by user user1
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/gitcheckout
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Checkout)
[Pipeline] withEnv
[Pipeline] {
[Pipeline] echo
==================== env.GIT_PASSWORD = gittokenvalue ====================
[Pipeline] echo
==================== env.GIT_USERNAME = gituser ====================
[Pipeline] echo
==================== env.GIT_BRANCH = main ====================
[Pipeline] script
[Pipeline] {
[Pipeline] withCredentials
Masking supported pattern matches of $gittokenvalue
[Pipeline] {
[Pipeline] checkout
The recommended git tool is: NONE
using credential credential-test
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/gitcheckout/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/gituser/CreateRepoTest.git # timeout=10
Fetching upstream changes from https://github.com/gituser/CreateRepoTest.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials credential-test
 > git fetch --tags --force --progress -- https://github.com/gituser/CreateRepoTest.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse origin/main^{commit} # timeout=10
Checking out Revision 8f1eff8fe6a5f7f32872036e01f905237d5ac310 (origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 8f1eff8fe6a5f7f32872036e01f905237d5ac310 # timeout=10
Commit message: "upload test- 2nd update"
 > git rev-list --no-walk 8f1eff8fe6a5f7f32872036e01f905237d5ac310 # timeout=10
Cleaning workspace
 > git rev-parse --verify HEAD # timeout=10
Resetting working tree
 > git reset --hard # timeout=10
 > git clean -fdx # timeout=10
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] sh
+ pwd
/var/lib/jenkins/workspace/gitcheckout
[Pipeline] sh
+ ls -al
total 20
drwxr-xr-x 5 jenkins jenkins 4096 12월 12 23:55 .
drwxr-xr-x 8 jenkins jenkins 4096 12월 12 23:10 ..
drwxr-xr-x 3 jenkins jenkins 4096 12월 12 23:23 dir1
drwxr-xr-x 8 jenkins jenkins 4096 12월 12 23:57 .git
drwxr-xr-x 3 jenkins jenkins 4096 12월 12 23:56 TestDir
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```




---
layout: default
title: Github Page On Local
parent: Git & Github
grand_parent: Cloud
permalink: /docs/cloud/gitgithub-githubpagelocal/
nav_order: 81
---

# Github Page 로컬에서 실행하기 - Jekyll
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}



---

{: .note }
깃허브 페이지를 작성 후 로컬에서 테스트할 수 있는 환경을 구성하고자 한다. 깃허브에 Push 후 확인할 수 있지만, 오타가 기타 오류가 발생할 경우 재업로드 해야 하는 불편함이 따르므로 Push 이전에 항상 로컬에서 테스트를 하면 이러한 불편함을 없앨 수 있다.

------


## 1. 루비(Ruby) 설치

- URL : https://rubyinstaller.org/downloads/
    - Ruby+Devkit 2.7.4-1 (x64) : 3.0 지원안하므로 2.X 로 설치한다.
    
- 버전 확인 : ruby -v
    

------

## 2. 지킬(Jekyll) 설치

- Command Prompt with Ruby를 실행
    - gem install jekyll
    - gem install bundler


- 샘플블로그 체크
    - jekyll new HelloBlog
	- cd HelloBlog
	- bundle exec jekyll serve
	    - http://127.0.0.1:4000/
	

------


## 3. 로컬 환경 접속

- 깃허브 페이지 폴더로 이동한다.
- 인코딩 에러 발생시 다음의 코드를 실행한다.
    - chcp 65001
- 지킬을 실행한다.
    - jekyll serve
    - 브라우저를 열어 http://127.0.0.1:4000/로 접속하면 로컬 상에서 블로그가 구현된 결과를 살펴 볼 수 있다.
	


------

## 4. 업로드 


>포스트 업로드하는 것은 일반적인 소스코드 업로드하는 것과 동일하다. VSCODE 에서 직접 업로드 하는 방법이 있으며, 작업의 반복적인 부분이 있어 쉘 스크립트로 자동화가 가능하며 자동화하는 방법은 다음과 같다.

- 쉘스크립트
    - 프로젝트 폴더에 post_upload.sh 파일을 만들고 다음과 같이 내용을 입력한다.

        ```sh
        echo `git status`
        echo `git add _posts/*`
        echo `git commit -m "post upload"`
        echo `git push origin master`
        ```

    - 실행 명령어는 다음과 같다.

        ```shell
        $ sh post_upload.sh
        ```

- 로컬 실행 : C:\park.github.io>bundle exec jekyll serve

    ```shell
    C:\park.github.io>git add .

    C:\park.github.io>git commit -m "3rd blog page test"

    C:\park.github.io>git push -u origin master
    ```



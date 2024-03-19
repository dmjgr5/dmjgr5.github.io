---
layout: default
title: Java Heap 분석
parent: Linux
grand_parent: OS Systems
permalink: /docs/os-systems/linux-javaheap/
nav_order: 35
---

# Jps, Jmap,Jhat 을 이용한 Java Heap 분석
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

{: .note}
OOM 오류 시 분석 가능한 방법을 찾아본다. [https://imasoftwareengineer.tistory.com/4](https://imasoftwareengineer.tistory.com/4) 를 참조하여 작성하였다.



## 테스트 코드 작성 

최대한 많은 크기로 객체 크기 증가 시킨다.

## jps 를 통해서 process id 확인

`jps`


## jmap 를 통한 힙 덤프 생성

`jmap -dump:live,file=<dump-file> <pid>`

## jhat 을 통한 로컬서버에서의 힙덤프 분석

`jhat -J-Xmx<Memory_SIZE> -port <PORT> <HEAP_DUMP_PATH>`

## 참조자료 대로 분석






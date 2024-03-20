---
layout: default
title: Oracle 에서 테이블과 데이터 복사하기
parent: Database
nav_order: 23
---

# Oracle 에서 테이블과 데이터 복사하기 

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

----

오라클에서 DB 작업 시 테이블 단위로 스키마를 복사하거나, 데이터를 복사할 때 유용하다.
 

 
### 테이블 복사하기 스키마 & 데이터

```sh
CREATE TABLE 새로만들테이블명 AS
SELECT * FROM 복사할테이블명 [WHERE 절]
``` 
 
### 테이블 구조만 복사하기

```sh
CREATE TABLE 새로만들테이블명 AS
SELECT * FROM 복사할테이블명 WHERE 1=2 [where절에 '참'이 아닌 조건을 넣어줌]
``` 
 
### 테이블은 이미 생성되어 있고 데이터만 복사 (테이블 구조가 동일할 때)

```sh
INSERT INTO 복사할테이블명 SELECT * FROM 테이블명 [WHERE 절]

--Sample SQL
INSERT INTO TB_BOARD_TEMP SELECT * FROM TB_BOARD;

``` 
 
### 테이블은 이미 생성되어 있고 데이터만 복사 (테이블 구조가 다를 때)

```sh
INSERT INTO 복사할테이블명 (NUM, TITLE, CONTENTS) SELECT NUM, TITLE, CONTENTS FROM 테이블명

--Sample SQL
INSERT INTO TB_BOARD_TEMP (NUM, TITLE, CONTENTS) SELECT NUM, TITLE, CONTENTS FROM TB_BOARD;
```
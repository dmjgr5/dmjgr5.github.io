---
layout: default
title: Oracle 에서 ALTER 이용하기
parent: Database
nav_order: 22
---

# Oracle 에서 ALTER 이용하기

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

----

오라클에서 DB 작업 시 테이블, 컬럼 또는 제약조건 명을 변경할 일이 생긴다. ALTER 구문을 활용해 보자.

참조자료 : [https://m.blog.naver.com/regenesis90/222200592868](https://m.blog.naver.com/regenesis90/222200592868)


## 테이블명 변경

```sql
alter table 테이블이름A
      rename to 테이블이름B;
```

## 컬럼명 변경

```sql
alter table 테이블이름A
      rename column 컬럼이름K to 컬럼이름L;
```

## 제약조건명 변경

```sql
alter table 테이블이름A
      rename constraints 제약조건이름X to 제약조건이름Y;
```


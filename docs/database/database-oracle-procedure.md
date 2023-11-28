---
layout: default
title: Oracle Proceduer 생성/실행
parent: Database
nav_order: 20
---

# Oracle Proceduer 생성/실행

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

----

오라클에서의 프로시저 생성 방법을 예제로 정리하고자 한다.


## 테이블 생성 및 데이터 추가

```sql
CREATE TABLE pl_student (
     hakbun NUMBER PRIMARY KEY,
     name VARCHAR2(34) NOT NULL, 
     kor NUMBER, eng NUMBER, math NUMBER
    );
```

```sql
INSERT INTO pl_student VALUES(1,'홍길동',90,90,100);
INSERT INTO pl_student VALUES(2,'박문수',85,80,75);
```

### 테이블 데이터 확인


![](/assets/images/oracle-proceduer1.png)

### 프로시저 작성

```sql
CREATE OR REPLACE PROCEDURE studentInsert(
    pName pl_student.name%TYPE,
    pKor pl_student.kor%TYPE,
    pEng pl_student.eng%TYPE,
    pMath pl_student.math%TYPE
)
IS
BEGIN
    INSERT INTO pl_student VALUES(
        (SELECT NVL(MAX(hakbun)+1,1) FROM pl_student),
       pName,pKor,pEng,pMath 
    );
    COMMIT;
END;
```

### 프로시저 실행

```sql
CALL studentInsert('돌쇠',30,97,46);
```



![](/assets/images/oracle-proceduer2.png)


### SQL 실행계획 보기
 

DBeaver 의 경우는 `마우스 오른쪽 클릭 > Excute > Explain Execution Plan` 을 클릭한다.



![](/assets/images/oracle-proceduer3.png)

또 다른 방법으로 아래와 같이 확인할 수 있다.

```sql
EXPLAIN PLAN -- EXPLANIN  PLAN 선언부
SET STATEMENT_ID = 'PLAN1' INTO PLAN_TABLE -- SQL에 PLAN1이라는 ID 부여
FOR
SELECT * FROM PL_STUDENT ps ;

SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY -- PLAN_TABLE에 저장된 실행계획 조회
('PLAN_TABLE','PLAN1','ALL'));
```


![](/assets/images/oracle-proceduer4.png)

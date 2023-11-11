---
layout: default
title: Proc*C
parent: Programming Language
nav_order: 8
---

# Proc*C
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
 

 


![example](/assets/images/proc.png)

{: .note}
ProC is a uncommon programming language. I have known the language only with C and C# related to series of C world. ProC could be available to use with SQL in application sources, let me summarize the basic commands and a few samples.

 
 
### DECLARE
DECLARE section be used between BEGIN and END.

```sql
EXEC SQL BEGIN DECLARE SECTION
    /***********************

       declare host variables                                     

    ***********************/ 
EXEC SQL END DECLARE SECTION;

```

### SELECT
The variable number of SELECT and INTO section should be identical.

```sql

EXEC SQL SELECT name, age, addr
               INTO :names, :ages, :addrs    //requries : in case of using SELECT
               FROM ex
               where key = :key;

```


### DML - INSERT, UPDATE, DELETE
DML types are easy to use by adding EXEC SQL before normal SQL queries.


```sql

EXEC SQL INSERT INTO ex (name, age) VALUES (:names, :ages);
EXEC SQL UPDATE ex SET name = :names WHERE key = :key;
EXEC SQL DELETE FROM ex WHERE key = :key;

```

### CURSOR - DECLARE, OPEN, FETCH, CLOSE
It is mandatory not to use INTO phrase in SELECT section. FETCH is used with FOR section normally, therefore we can use a condition like WHENEVER to escape FOR section. In order to do release the resource after FETCH, CLOSE should be used.

```sql

EXEC SQL DECLARE ex_s CURSOR FOR // declare a cursor
        SELECT name, age
        FROM ex
        WHERE key = :key
        ORDER BY key;

EXEC SQL OPEN ex_s;              // open with cursor name

EXEC SQL FETCH ex_s              // fetch data, same the number of SELECT and FETCH INTO phrase
        INTO :name, :age;

EXEC SQL CLOSE ex_s;             // 

```





 
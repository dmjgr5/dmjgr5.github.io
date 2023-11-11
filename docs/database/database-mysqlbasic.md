---
layout: default
title: MySQL 기본
parent: Database
nav_order: 10
---

# MySQL 기본  
{: .no_toc }

![example](/assets/images/mysql.png)


----



### MySQL Installation
- In this turotial, Bitnami APM used.


### Access MySQL using MySQL Monitor
To access MySQL, go to `C:\Bitnami\wampstack-7.3.11-0\mysql\bin` in CMD, then command `mysql -uroot -p`. After input password, you can access MySQL server.


### Create Schema (Database)
- create : `CREATE DATABASE openturorials;`
- check database list : `SHOW DATABASES;`
- use and select database, command `USE openturorials;`
- delete : `DROP DATABASE openturorials;`


### Create Table
```sql
mysql> CREATE TABLE topic(
    ->   id INT(11) NOT NULL AUTO_INCREMENT,
    ->   title VARCHAR(100) NOT NULL,
    ->   description TEXT NULL,
    ->   created DATETIME NOT NULL,
    ->   author VARCHAR(30) NULL,
    ->   profile VARCHAR(100) NULL,
    ->   PRIMARY KEY(id));
Query OK, 0 rows affected, 1 warning (0.18 sec)
```

### Show Table
```sql
mysql> SHOW tables;
+-------------------------+
| Tables_in_openturorials |
+-------------------------+
| topic                   |
+-------------------------+
1 row in set (0.02 sec)
```

### Desc Table

```sql
mysql> DESC topic;
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| title       | varchar(100) | NO   |     | NULL    |                |
| description | text         | YES  |     | NULL    |                |
| created     | datetime     | NO   |     | NULL    |                |
| author      | varchar(30)  | YES  |     | NULL    |                |
| profile     | varchar(100) | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
6 rows in set (0.01 sec)
```


### CRUD 

```sql
mysql> INSERT INTO topic (title, description, created, author, profile) VALUES ('MySQL','MySQL is ...', NOW(), 'egoing', 'developer' )
    -> ;
Query OK, 1 row affected (0.02 sec)

mysql> SELECT * FROM topic;
+----+-------+--------------+---------------------+--------+-----------+
| id | title     | description  | created                  | author  | profile     |
+----+-------+--------------+---------------------+--------+-----------+
|  1 | MySQL | MySQL is ... | 2019-12-16 23:08:25 | egoing | developer |
+----+-------+--------------+---------------------+--------+-----------+
1 row in set (0.00 sec)
```

```sql
mysql> UPDATE topic SET description='Oracle is ...', title = 'Oracle' where id = 2;
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

```sql
mysql> DELETE FROM topic WHERE id = 5;
Query OK, 1 row affected (0.02 sec)
```

### LIMIT

To limit output size, add LIMIT at the end of SQL

```sql
mysql> SELECT * from topic;
+----+------------+-------------------+---------------------+--------+---------------------------+
| id | title      | description       | created             | author | profile                   |
+----+------------+-------------------+---------------------+--------+---------------------------+
|  1 | MySQL      | MySQL is ...      | 2019-12-16 23:08:25 | egoing | developer                 |
|  2 | ORACLE     | ORACLE is ...     | 2019-12-16 23:09:47 | egoing | developer                 |
|  3 | SQL Server | SQL SERVER is ... | 2019-12-16 23:10:44 | duru   | data administrator        |
|  4 | PostgreSQL | POSTgreSQL is ... | 2019-12-16 23:11:31 | taeho  | data scientist, developer |
|  5 | MongoDB    | MongoDB is ...    | 2019-12-16 23:12:05 | egoing | developer                 |
+----+------------+-------------------+---------------------+--------+---------------------------+
5 rows in set (0.00 sec)
```

```sql
mysql> SELECT * from topic LIMIT 2;
+----+--------+---------------+---------------------+--------+-----------+
| id | title  | description   | created             | author | profile   |
+----+--------+---------------+---------------------+--------+-----------+
|  1 | MySQL  | MySQL is ...  | 2019-12-16 23:08:25 | egoing | developer |
|  2 | ORACLE | ORACLE is ... | 2019-12-16 23:09:47 | egoing | developer |
+----+--------+---------------+---------------------+--------+-----------+
2 rows in set (0.00 sec)
```


### Join

```sql
mysql> SELECT * FROM topic LEFT JOIN author ON topic.author_id = author.id;
+----+------------+-------------------+---------------------+-----------+------+--------+---------------------------+
| id | title      | description       | created             | author_id | id   | name   | profile                   |
+----+------------+-------------------+---------------------+-----------+------+--------+---------------------------+
|  1 | MySQL      | MySQL is...       | 2018-01-01 12:10:11 |         1 |    1 | egoing | developer                 |
|  2 | Oracle     | Oracle is ...     | 2018-01-03 13:01:10 |         1 |    1 | egoing | developer                 |
|  3 | SQL Server | SQL Server is ... | 2018-01-20 11:01:10 |         2 |    2 | duru   | database administrator    |
|  4 | PostgreSQL | PostgreSQL is ... | 2018-01-23 01:03:03 |         3 |    3 | taeho  | data scientist, developer |
|  5 | MongoDB    | MongoDB is ...    | 2018-01-30 12:31:03 |         1 |    1 | egoing | developer                 |
+----+------------+-------------------+---------------------+-----------+------+--------+---------------------------+
5 rows in set (0.00 sec)
```

### MySQL Client
Mostly MySQL WORKBENCH recommended. It is easy to use SQL because it is GUI.
[https://dev.mysql.com/downloads/file/?id=490464](https://dev.mysql.com/downloads/file/?id=490464)



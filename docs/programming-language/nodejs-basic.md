---
layout: default
title: Node.js 기본 개념
parent: Node.js
grand_parent: Programming Language
permalink: /docs/programming-language/
nav_order: 10
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


​node.js 의 종류와 사용방법에 대해 정리한다.
 

{: .important-title }
> 참고 자료
>
> [한시간만에 끝내는 Node.js 입문](https://www.youtube.com/watch?v=toLDNN4FQv0)


## Node.js 의 특징

-   `javaScript` 를 이용한다.
-   엄청나게 많은 패키지를 이용할 수 있다.

## 모듈 설치 및 실행

`https://www.npmjs.com/` 에서 원하는 모듈을 찾는다.

`npm install [모듈명]` 으로 아래와 같이 설치한다.  
nodemailer 라는 모듈을 예시로 든다.

```bash
D:\99_nodeJs>npm install nodemailer

added 1 package, and audited 2 packages in 1s

found 0 vulnerabilitie
```

아래 그림과 같이 node\_modules, package json 파일 등이 생성된다.


![](/assets/images/nodejs1.png)



`require` 를 이용해서 패키지를 불러온다.

`const nodemailer = require('nodemailer');`

## 실습 예제

### 이메일 전송하기

`nodemailer` 모듈을 사용한다.

```javascript
const nodemailer = require('nodemailer');  

const email= {
    host: "smtp.mailtrap.io",
    port: 2525,
    auth: {
      user: "80e8c610c6dace",
      pass: "aad60e1f687512"
    }
};


const send = async (option) => {
    nodemailer.createTransport( email ).sendMail(option, (error, info) => {
        if(error){
            console.log(error)
        } else {
            console.log(info);
            return info.response;
        }
    });
};

let email_data = {
    from: "dmjgr5@gmail.com",
    to: "dmjgr5@gmail.com",
    subject: "테스트 메일입니다.",
    text: "node.js 테스트입니다. "
}

send(email_data);
```

### 웹서버 만들기

`express` 모듈을 사용한다.

```javascript
const express = require('express'); // 모듈 호출
const app = express();

const server = app.listen(3000, () => { //3000 번 포트로 지정
console.log('start server : localhost:3000');
});

app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.engine('html', require('ejs').renderFile);

app.get('/', function (req, res) {
    res.render('index.html');
  })


app.get('/about', function (req, res) {
    res.render('about.html');
  })
```

\`node test.js'

### MySQL 사용하기

`npm install mysql --save` 모듈을 사용한다.

```javascript
const express = require('express'); // 모듈 호출
const app = express();

const server = app.listen(3000, () => { //3000 번 포트로 지정
console.log('start server : localhost:3000');
});



var mysql      = require('mysql');
var pool  = mysql.createPool({
host     : '127.0.0.1',
user     : 'root',
password : '',
database : 'yse_spring_mvc'
});



app.get('/db', function (req, res) {
    pool.getConnection(function(err, connection) {
        if (err) throw err; // not connected!

        // Use the connection
        connection.query('select 1+1 sum from dual', function (error, results, fields) {


            res.send(JSON.stringify(results));
            console.log('results', results);

          // When done with the connection, release it.
          connection.release();

          // Handle error after the release.
          if (error) throw error;

          // Don't use the connection here, it has been returned to the pool.
        });
      });

})
```
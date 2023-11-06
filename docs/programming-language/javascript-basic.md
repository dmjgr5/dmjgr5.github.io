---
layout: default
title: Java Script 기본 개념
parent: Java Script
grand_parent: Programming Language
permalink: /docs/programming-language/
nav_order: 10
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


 ​
## 자바스크립트 목적 및 주요 기능
​
-   input 창에 특정 값을 넣어주는 용도(input 태그)
-   특정 element(DOM) 를 보여주고 안보여주는 용도(select 태그)
-   `JavaScript` 는 body 제일 밑에 쓴다. `css` 의 경우는 head 마지막에 쓴다.
​
### Variable
​
-   `let` (added in ES6) : `var` 는 사용하지 말것!, 숫자는 `number` 를 사용할 수 있음.
-   `const`
-   template literals
-   숫자 + 문자는 숫자로 된다. ex( `'1' + 2 ==> 3`)
​
### Equality
​
-   Loose equality : `==` , `!=` 를 사용하며 타입이 다르더라도 값이 같으면 같다고 본다.
-   Strict equality : `===`, `!==` 값이 동일해도 타입이 다르면 다른거다.
​
### Function
​
`object` 타입이기 때문에 파라미터로도 전달될 수 있다.
​
-   defalut parameter : 인자에 값을 넣는다.
-   `function showMessage(message, from = 'defaultname') { ........ }`
-   Rest parameters 점 3개
-   `function showMessage(...args) { for(let i = 0; i < args.length; i++) { console.log(args[i]; } }`
​
```javascript
// 함수(익명함수)를 선언함과 동시에 변수에 할당이 된다.
const print = function [함수이름] () { => [함수이름]  은 디버깅 용도로 추가할 수도 있다.
    console.log('print');
}
print()
​
// 다른 변수에 할당이 가능하다.
const printAgain = print;
printAgain();
```
​
### Arrow Function
​
항상 이름이 없는 함수이다
​
```javascript
// print 와 printArrow 는 동일하다.
const print = function() {
    console.log('print');
}
print();
​
​
const printArrow = () => {
    console.log('print');
}
printArrow();
```
​
### Object
​
object 는 key 와 value 의 집합체이다.
​
### Array APIs (map, filter, join)
​ 
![](/assets/images/js1.png)

### JSON
​
-   `JSON.stringfy` : Object to JSON
-   `JSON.parse` : JSON to Object



​
## 주요 특징

​
### 선언형 var, let, const
​
아래 3가지만 기억하자.
​
-   `var`  
    중복 선언을 허용한다.
-   `var x = 1; x = x + 2; document.getElementById("test").value = x; //3`
​
var x = 2;  
document.getElementById("test").value = x; //2
​
-   `let`  
    중복 선언을 허용하지 않는다.
​
```javascript
let y = 1;  
y = y + 2;
​
let y = 2; // 동일 변수는 허용하지 않는다.
```
​
-   `const`  
    재정의 불가하다.
​
### 데이터 타입
​
숫자, 소수점 숫자, 문자, 참/거짓, 오브젝트, 배열 등등이 있다. 아래와 같이 선언된다.
​
```javascript
​
let a = 1;  
a = 1.234;  
a = "apple"  
a = true;
​
// object
​
let obj ={  
name:"", ////////////////////// 선언시점에 키를 선언안해도 된다!!  
city:"" ////////////////////// 선언시점에 키를 선언안해도 된다!!  
};
​
obj.name = $("#name").val();  
obj.city = $("#city").val();
​
console.log(obj);
​
// 배열
let city = [
{code:"02", name:"서울"},
{code:"064", name:"제주"}
];
```
​
다중 정의가 가능하다. Object 안에 Object 또는 배열을 담을 수 있다.
​
```javascript
let city = \[  
{code:"02", name:"서울"},  
{code:"064", name:"제주"}  
\];
```
​
### 반복문
​
```javascript
​
for(let i=0; i<city.length; i++){  
////////////////////////// 조건문  
if(i==0) {  
$('#city').append('' + city\[i\].name + '');  
}  
else if(1==0) {  
///  
}  
else {  
$('#city').append('' + city\[i\].name + '');  
}  
}
```
​
## 전체 실습 코드

 

<div class="container">
    <div>이름: <input type="text" id="name" /></div>
    <div>지역: <select id="city" onchange="changeCity();">
            <!-- <option value="02">서울</option>
            <option value="064">제주</option> -->
        </select>
    </div>
​
    <div>동네: 
        <select id="region_02">
            <option value="">강남</option>
            <option value="">서초</option>
        </select>
        <select id="region_064" style="display:none;">
            <option value="">제주시</option>
            <option value="">서귀포시</option>
        </select>
    </div>
​
    <div>test <input type="text" id="test" value="" /></div>
​
    <button type="button" onclick="regist();">등록</button>
​
</div>
​
<script src="https://code.jquery.com/jquery-3.6.3.min.js" integrity="sha256-pvPw+upLPUjgMXY0G+8O0xUf+/Im1MZjXxxgOcBQBXU=" crossorigin="anonymous"></script>
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"
    integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj"
    crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"
    integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN"
    crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.min.js"
    integrity="sha384-w1Q4orYjBQndcko6MimVbzY0tgp4pWB4lZ7lr30WKz0vr/aWKhXdBNmNb5D92v7s"
    crossorigin="anonymous"></script>
​
<!-- 스크립트 시작 -->
<script>
​
​
​
    function regist() {
        ////////////////////////// 본 document 에 있는 element 중에서 id 로 city 인 값을 가져와라.
        alert(document.getElementById("city").value);
​
        //jQuery 예제
        ////////////////////////// jQuery 는 $ 를 쓰고, id 는 # 으로 표현한다.
        alert($("#name").val())
​
        let obj ={
            name:"",
            city:""
        }; // 오브젝트
​
​
        obj.name = $("#name").val();
        obj.city = $("#city").val();
​
        console.log(obj);
​
    }
​
    function changeCity() {
        //let city = document.getElementById("city").value;
        //document.getElementById("region_02").style.display = "none"; // "" 또는 "none"
        //document.getElementById("region_064").style.display = "none"; // "" 또는 "none"
​
        //document.getElementById("region_"+city).style.display = ""; // "" 또는 "none"
​
        ////////////////////////// jQuery 라면
        let city = $('#city').val();
        $('#region_02').hide();
        $('#region_064').hide();
        $('#region_' + city).show();
​
    }
​
​
    function loadCity() {
        let city = [
            {code:"02", name:"서울"},
            {code:"064", name:"제주"}
        ];
​
​
        //$('#city').append('<option value="02" selected> 서울 </option>');
       // $('#city').append('<option value="064" > 제주 </option>');
​
        //////////////////////////// 반복문
        for(let i=0; i<city.length; i++){
            ////////////////////////// 조건문
            if(i==0) {
                $('#city').append('<option value="' + city[i].code + '" selected >' + city[i].name + ' </option>');
            } 
            else if(1==0) {
                ///
            }
            else {
                $('#city').append('<option value="' + city[i].code + '"  >' + city[i].name + ' </option>');
            }
        }
​
​
    }
​
​
​
    loadCity();// 강제로 실행
​
    //var
    var x = 1;
    x = x + 2;
    document.getElementById("test").value = x; //3
​
    var x = 2;
    document.getElementById("test").value = x; //2
​
​
​
​
    ////////////////////////// 형타입은 let 으로 일관된다.
    let a = 1;
    a = 1.234;
    a = "apple"
    a = true;
​
​
    ////////////////////////// 오브젝트
    let obj ={
        name:"",
        city:""
    };  
​
​
​
​
</script>

​
``` javascript

<div class="container">
    <div>이름: <input type="text" id="name" /></div>
    <div>지역: <select id="city" onchange="changeCity();">
            <!-- <option value="02">서울</option>
            <option value="064">제주</option> -->
        </select>
    </div>
​
    <div>동네: 
        <select id="region_02">
            <option value="">강남</option>
            <option value="">서초</option>
        </select>
        <select id="region_064" style="display:none;">
            <option value="">제주시</option>
            <option value="">서귀포시</option>
        </select>
    </div>
​
    <div>test <input type="text" id="test" value="" /></div>
​
    <button type="button" onclick="regist();">등록</button>
​
</div>
​
<script src="https://code.jquery.com/jquery-3.6.3.min.js" integrity="sha256-pvPw+upLPUjgMXY0G+8O0xUf+/Im1MZjXxxgOcBQBXU=" crossorigin="anonymous"></script>
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"
    integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj"
    crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"
    integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN"
    crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.min.js"
    integrity="sha384-w1Q4orYjBQndcko6MimVbzY0tgp4pWB4lZ7lr30WKz0vr/aWKhXdBNmNb5D92v7s"
    crossorigin="anonymous"></script>
​
<!-- 스크립트 시작 -->
<script>
​
​
​
    function regist() {
        ////////////////////////// 본 document 에 있는 element 중에서 id 로 city 인 값을 가져와라.
        alert(document.getElementById("city").value);
​
        //jQuery 예제
        ////////////////////////// jQuery 는 $ 를 쓰고, id 는 # 으로 표현한다.
        alert($("#name").val())
​
        let obj ={
            name:"",
            city:""
        }; // 오브젝트
​
​
        obj.name = $("#name").val();
        obj.city = $("#city").val();
​
        console.log(obj);
​
    }
​
    function changeCity() {
        //let city = document.getElementById("city").value;
        //document.getElementById("region_02").style.display = "none"; // "" 또는 "none"
        //document.getElementById("region_064").style.display = "none"; // "" 또는 "none"
​
        //document.getElementById("region_"+city).style.display = ""; // "" 또는 "none"
​
        ////////////////////////// jQuery 라면
        let city = $('#city').val();
        $('#region_02').hide();
        $('#region_064').hide();
        $('#region_' + city).show();
​
    }
​
​
    function loadCity() {
        let city = [
            {code:"02", name:"서울"},
            {code:"064", name:"제주"}
        ];
​
​
        //$('#city').append('<option value="02" selected> 서울 </option>');
       // $('#city').append('<option value="064" > 제주 </option>');
​
        //////////////////////////// 반복문
        for(let i=0; i<city.length; i++){
            ////////////////////////// 조건문
            if(i==0) {
                $('#city').append('<option value="' + city[i].code + '" selected >' + city[i].name + ' </option>');
            } 
            else if(1==0) {
                ///
            }
            else {
                $('#city').append('<option value="' + city[i].code + '"  >' + city[i].name + ' </option>');
            }
        }
​
​
    }
​
​
​
    loadCity();// 강제로 실행
​
    //var
    var x = 1;
    x = x + 2;
    document.getElementById("test").value = x; //3
​
    var x = 2;
    document.getElementById("test").value = x; //2
​
​
​
​
    ////////////////////////// 형타입은 let 으로 일관된다.
    let a = 1;
    a = 1.234;
    a = "apple"
    a = true;
​
​
    ////////////////////////// 오브젝트
    let obj ={
        name:"",
        city:""
    };  
​
​
​
​
</script>
```
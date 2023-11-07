---
layout: default
title: Vue.js 기본 개념
parent: Vue.js
grand_parent: Programming Language
permalink: /docs/programming-language/
nav_order: 10
---


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

> 참조 자료 : \[인스타그램을 만들며 배워보는 Vue.js 3 완벽 가이드\] ([https://codingapple.com/course/vue-js/](https://codingapple.com/course/vue-js/))

## Vue 설치

## 데이터 바인딩

## 반복문

## 이벤트 핸들러

```
<button @mouseover ="신고수--" @click="신고수++">허위매물신고</button> <span>신고수 : {{신고수}}</span>
```

## 모달창 만들기

모달창을 미리 만들어 클릭 시 상태조건을 변경한다.

```
<div class="black-bg" v-if="모달창열렸니 == true">
<div class="white-bg">
  <h4>상세페이지임</h4>
  <p>상세페이지 내용임</p>
</div>
</div>
```

## export defalut

### 데이터 부분

```javascript
export default [{
    id : 0,
    title: "Sinrim station 30 meters away",
    image: "https://codingapple1.github.io/vue/room0.jpg",
    content: "18년 신축공사한 남향 원룸 ☀️, 공기청정기 제공",
    price: 340000
    },
    {
```

### 가져오는 부분

```javascript

import data from './assets/oneroom.js';

export default {
  name: 'App',
  data() {
    return {
      원룸들 : data,
```

```javascript
    <img :src="원룸들[2].image" class="room-img">
    <h4 @click="모달창열렸니 = true">{{원룸들[2].title}} </h4>
    <p>{{원룸들[2].price}} 원 </p>
```
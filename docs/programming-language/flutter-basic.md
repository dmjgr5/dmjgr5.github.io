---
layout: default
title: Flutter 기본 개념
parent: Flutter
grand_parent: Programming Language
permalink: /docs/programming-language/flutter-basic/
nav_order: 10
---

# Flutter 기본 개념
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


> 앱을 구축하는데 있어서 주된 걸림돌인 Android, iOS 등의 상이한 아키텍쳐가 있었으나, 구글에서 개발한 `Flutter` 는 이를 보완하고 보다 직관적으로 개발을 할 수 있게 한다. 기본적인 내용 및 개발 시 참조해야 할 만한 사항을 정리하고자 합니다..

---
 
## 주요 에러 해결 방법

## `don't support null safety: - package:english_words` 에러
- Run --> Edit Configurations --> Add Additional Run args 에서 `--no-sound-null-safety` 를 추가한다.

## 코드 정렬
- 단축키 Ctl+Alt+L을 누르면 아래와 같이 자동정렬이 됩니다. 

## Flutter Layout 학습 사이트
- [https://medium.com/flutter-community/flutter-layout-cheat-sheet-5363348d037e](https://medium.com/flutter-community/flutter-layout-cheat-sheet-5363348d037e)

## Const 와 Final 의 차이점
- Cont 변수 : 컴파일 시 상수가 된다. 런타임 시에도 변하지 않는다. (Compile-time Constant)
- Final 변수 : 런타임 시 상수가 된다. 변경 시, reBuild 되어야 한다. (Run-time Constant)


## 자료구조 종류

- List, Set

## Thread
- 프로세스 내에서 실행되는 흐름의 단위
- `Dart` 는 싱글스레드로 운영되는 언어


## Future, Async, await
- Future 클래스는 비동기 작업을 할 때 사용
- Future 는 일정 소요시간 후에 실제 데이터나 에러를 반환
- async 클래스는 await 메서드를 가지고 있음
  - await 로 선언된 메서드는 응답이 처리될 때까지 대기
- 예제

```python

import 'dart:io';

void main() {
  showData();
}

void showData() async {
  startTask();
  String account = await accessData();
  fetchData(account);
}

void startTask() {
  String info1 = '요청수행 시작';
  print(info1);
}

Future<String> accessData() async {
  String account = '';

  Duration time = Duration(seconds: 3);

  if (time.inSeconds > 2) {
    // sleep(time);
    await Future.delayed(time, () {
      account = '8500만원';
      print(account);
    });
  } else {
    String info2 = '데이터를 가져왔습니다';
    print(info2);
  }
  return account;
}

void fetchData(String account) {
  String info3 = '잔액은 $account 원입니다..';
  print(info3);
}

```

`출력`
```bash
요청수행 시작
8500만원
잔액은 8500만원 원입니다..
```


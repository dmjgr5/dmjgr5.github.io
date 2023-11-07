---
layout: default
title: gcc 컴파일하기
parent: C, C#
grand_parent: Programming Language
permalink: /docs/programming-language/gcc/
nav_order: 10
---



리눅스에서는 간단하게 `gcc` (GNU compiler collection) 컴파일러를 이용하여 컴파일 할 수 있다.
예제로 아래의 코드를 작성한 후 gcctest.c 파일로 저장한다.
​
```c
#include<stdio.h>
​
int main() {
        printf("Hello, world\n");
        return 0;
}
```
​
`gcc -o [실행파일명] [소스파일명]` 명령어를 사용하여 아래와 같이 실행파일을 생성할 수 있다.

​
```bash
root@master:~# ll | egrep gcc
-rw-r--r--  1 root root     72 Jan 18 13:50 gcctest.c
root@master:~#

root@master:~# gcc -o gcctest.out gcctest.c
root@master:~#

root@master:~# ll | egrep gcc
-rw-r--r--  1 root root     72 Jan 18 13:50 gcctest.c
-rwxr-xr-x  1 root root   8304 Jan 18 13:51 gcctest.out*
root@master:~#

root@master:~# ./gcctest.out
Hello, world
root@master:~#
```
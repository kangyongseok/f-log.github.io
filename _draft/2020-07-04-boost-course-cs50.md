---
title: "부스트코스 - 모두를 위한 컴퓨터 과학 (CS50 2019)"
categories: 
  -  boostcourse
tags: 
    - develop
    - study
    - edwith
    - boostcourse
    - cs50
    - C
toc: true
toc_sticky: true
comments:  true
---

## C 언어
```c
#include <stdio.h> // 입출력 관련 기능들을 사용하기위해 불러오는 함수모움
int main(void) 
{
    printf("hello, world")
}
```

### 문자열

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string answer = get_string("What's your name?\n");
    printf("hello %s\n", answer);
}

```

```console
make string
```

## 조건문과 루프
```c
if (x < y)
{
    printf('x 는 y 보다 더 작다\n');
}
else if (x > y)
{
    printf('x 는 y 보다 더 크다\n');
}
else
{
    printf('x 와 y 는 같다');
}

// 무한 반복
while(true)
{
    printf('hello, world\n');
}

// i 가 0부터 50이 될떄까지 반복
int i = 0
while(i < 50)
{
    printf('hello, world\n');
    i++
}

// 위의 while 문과 같은 결과가 나온다.
for(int i = 0; i < 50; i++)
{
    printf('hello, world\n');
}
```
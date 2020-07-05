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
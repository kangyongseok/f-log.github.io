---
title: "부스트코스 - 모두를 위한 컴퓨터 과학 (CS50 2019) - 배열"
categories: 
  -  boostcourse
tags: 
    - develop
    - study
    - edwith
    - boostcourse
    - cs50
    - C
    - compile
toc: true
toc_sticky: true
comments:  true
---

## 컴파일링
```c
#include <stdio.h>
#include <cs50.h>

int main(void)
{
    printf("hello, world!\n");
}
```

```terminal
$ clang -o hello hello.c -lcs50
or
$ make hello
```

지금까지 위와같은 형태로 계속 실습을 하면서 명령어를 통해 컴파일링을 하고 실행을 했었는제 저 명령어가 정확히 어떤 방식으로 동작하는지 생각해볼 필요성이 있다.  

처음강의에서 컴퓨터는 0과 1로만 이루어져있다고 했다. 그렇다면 c 에서 사용하는 소스코드와 명령어를 어떻게 컴퓨터는 받아들일까?  

```terminal
$ clang -o hello hello.c -lcs50
or
$ make hello
```

바로 이 컴파일 실행 명령어를 통해 작성한 소스코드는 어셈블리어로 대체되고 이 어셈블리어는 다시 0과 1로 이루어진 데이터로 변형된다. 그리고 `#include`로 불러와진 함수들의 모음을 가진 라이브러리 파일들도 함께 포함시켜 컴파일하면서 하나의 출력파일로 만들고 컴퓨터는 이를 읽어들일 수 있게된다.

## 문자열
문자열을 가진 변수들은 각각 메모리에 저장되는데 문자열같은경우 문자열이 끝났다 라는것을 구분하기위해 문자열 마지막에 항상 null 을 나타내는 0 이 포함된다.
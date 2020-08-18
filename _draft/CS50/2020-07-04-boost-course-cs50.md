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

## C 기초

해당 강의에서는 클라우드기반의 툴을 사용한다.  
`sandbox.cs50.io` C 언어를 학습하는데 있어 유용한 보조도구들을 제공해 주는데 어차피 결국 실 사용은 저런 도구가 없는 날것의 C 언어를 사용해야 한다 생각해서 원래도 사용하고있던 vscode editor 에서 C 를 실행 할 수 있는 환경을 세팅하였다.
  
```c
#include <stdio.h> // printf() 를 사용하기위해 선언해 주어야함

int main(void) {
    printf("hello, world");
}
```

위의 코드를 실행하기 위해서는 컴퓨터가 이해 가능한 언어로 변환되어야 하는데 그것을 컴파일 이라고한다. 그리고 컴파일과 관련된 명령어들이 있는데 vscode 로 세팅을 해두면 단축키로 컴파일과 실행이 아주 편하게 가능하다.
```c
검색키워드
C vscode compile for mac or windows
```

위의 코드는 단순히 hello, world 를 출력하는 한 문장을 출력하기 위해 여러줄이 필요한데 저것이 C 를 실행하기위해 가장 최소한의 필요코드라고 볼 수있다.  

만약 어떤 수정사항이 발생한다면 컴파일을 다시 진행해줘야 제대로 실행되는걸 확인 할 수 있다.

## 문자열


## 조건문과 루프

## 자료형, 형식지정자, 연산자

## 사용자 정의 함수, 중첩 루프

## 하드웨어의 한계


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
#include <cs50.h> // 강좌를 진행하기 위해 정해진 곳에서 사용 가능한 함수들의 모음
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

## 사용자 정의함수
```c
#include <stdio.h>

void cough(void)
{
    printf("cough\n")
}

int main(void)
{
    cough()
    cough()
    cough()
}
```

사용자 정의함수인 `cough`를 만들어서 메인에서 출력되도록 만들 코드이다.  여기에서 문제점은 사용자 정의함수가 늘어날수록 `main` 은 점점 밑으로 내려가 나중엔 한참을 내려야 확인 할 수 있게 된다는 점이다.  

main 은 가장 맨 위에서 어떤 함수를 사용하고 결국 마지막에 출력하는게 무엇인지 알기위해 존재하는것이 좋다.

```c
#include <stdio.h>

int main(void)
{
    cough()
    cough()
    cough()
}

void cough(void)
{
    printf("cough\n")
}
```

그래서 사용자정의 함수인 `cough`를 밑으로 내린다음에 실행한다면 실패한다. 위에서부터 읽어내려오는데 cough 라는 함수가 main 이 나오기전에 존재하지 않기 때문이다.

```c
#include <stdio.h>

void cough(void)

int main(void)
{
    cough();
    cough();
    cough();
}

void cough(void)
{
    printf("cough\n");
}
```

해결방법은 사용자정의함수를 main 보다 먼저 호출해주는 방법이다. 위의 코드를 좀더 각자의 역할에 충실하도록 변경해보자

```c
#include <stdio.h>

void cough(int n)

int main(void)
{
    cough(3);
}

void cough(int n)
{
    for (int i = 0; i < n; i++)
    {
        printf("cough\n")
    }
}
```

위와같은 코드로 수정하면 훨씬 사용자 친화적이고 가독성이나 코드 사용 효율성에서 훨씬 유리한점을 가질 수 있다.
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
해당 강의에서는 sandbox 에 있는 ide 를 활용하여 예제 코드를 보여주는데 여기서 cs50 에서 만들어 제공하는 라이브러리에 있는 함수들을 몇가지 사용하는데 그중 하나가 `get_string()` 함수로 사용자가 입력한 string 데이터를 리턴해주는 함수이다.  

나는 이 도움 라이브러리를 사용하지않고 vscode 를 사용하기 떄문에 직접 만들어서 사용하려고 한다.
```c
char *get_string(char *text) {
    // 해당 강의가 진행되는 동안 100을 넘는 메모리 영역을 사용할 일이 없기에 최대를 100으로 지정
    static char str[100]; 
    printf("%s", text);
    scanf("%s", str);
    return (char*)str;
}
```

사용법도 조금 다른데 string 이라는 형식이 c 에서 기본으로 제공하는 형식이 아니기 때문에 직접 만든 get_string 함수는 아래와같이 사용해야 한다.

```c
int main(void) {
    char *text = get_string("입력하시오.");
    printf("%s", text);
    return 0;
}
```

get_string, printf 모두 함수이고 함수는 인자를 받을 수 있다. 직접만든 get_string 함수는 하나의 인자만을 받고 printf 함수는 두개의 인자를 받아 함수를 실행한다.  

`=` 부등호 기호는 무언가 같다 라고 사용될 수도 있겠지만 프로그래밍에서는 수학적인 의미로 사용된다. 오른쪽의 값이 왼쪽의 값에 입력되는 방식으로 사용된다.  

만약 코드에 어떤 문제가 있다면 컴파일 과정에서 에러를 출력하게되고 에러 구문을 해결하면 다시 정상적으로 컴파일이 된다.  

## 조건문과 루프
counter 변수를 증가시키는 다양한 방법
```c
int counter = 0; // counter 변수 초기화
counter = counter + 1; // 초기화하고 나서는 int형 선언은 추가로 하지 않아도 된다.
counter += counter; // 위의 식을 간단하게 줄인것
counter = counter ++; // 구문설탕으로 별다른 추가 기능은 없지만 좀더 간단하게 줄여 사용하는 방법
```

조건문 사용 문법
```c
if (x < y) // 조건이 들어가는 부분은 괄호 사용
{ // 참 조건 이후 실행할 코드부분은 중괄호 사용

} 
else
{
    // 조건이 거짓일때 실행할 코드 블록
}
```
조건이 여러개일때
```c
if (x < y) // 조건이 들어가는 부분은 괄호 사용
{ // 조건이 참인 이후 실행할 코드부분은 중괄호 사용

} 
else if (x == y) // = 이라는 부호는 오른쪽값을 왼쪽에 삽입할때 사용하기때문에 == 사용을 통해 같다 라는 표현을 프로그래밍에서는 사용한다.
{

}
else
{
    // 위의 두 조건을 모두 만족하지 않을때 실행할 코드 블록
}
```

루프 (반복문) 사용 문법
```c
while() // while 은 괄호안에 조건이 참인동안 중괄호 코드블록을 반복해서 실행한다.
{

}
```

i 가 50보다 작으면 실행하는 반복문
```c
int i = 0;
while (i < 50)
{
    printf("hello, world\n");
    i = i + 1; // or i++
}
```

또 다른 반복문법
```c
for (int i = 0; i < 50; i++)
{
    printf("hello, world\n");
}
```
for 반복문은 세개의 인자를 받는데 첫번째는 변수를 생성하면서 초기화를 진행하고 두번째로 조건문 세번쨰로 변수를 어떻게 수정할 것인지에 대한 코드를 입력 할 수 있다.   

위의 코드를 해석하면
1. i 를 정수형으로 선언하고 0을 저장한다 (초기화한다.)
2. i 가 50보다 작다면
3. i 는 1씩 증가한다.




## 자료형, 형식지정자, 연산자
**c 언어에 존재하는 다양한 타입 종류들**
```c
bool // true / false
char // 1개의 문자
double // 소수점 뒤에 포함하는 더 많은 데이터를 포함하는 실수를 나타냄
float // 실수를 나타냄 소수점이하까지의 숫자를 포함
int // 정수
long // int로 셀수없는 더 큰 수를 포함 할 수 있다.
string // "" 안에 들어가는 문자를 표현
...
```


**c 언어에서 사용하는 다양한 형식 지정자들**
```c
%c // char
%f // float, double
%i // int
%li // long
%s // string
```

이번 강의에서는 gwt_int 라는 함수를 사용하는데 이것또한 cs50에 포함된 사용자 함수이기 떄문에 vscode를 사용하는 나는 만들어서 사용하겠습니다.

```c
int get_int(char *text) {
    int getInt;
    printf("%s", text);
    scanf("%i", &getInt);
    return getInt;
}
```

**연산을 위한 예제코드** 
```c
#include <stdio.h>

int get_int(char *text); // 나중에 나오는 사용자 정의함수 사용법

int main(void)
{
    int age = get_int("What's your age?\n");
    int days = age * 365;
    printf("Your are at least %i days old\n", days);
}

int get_int(char *text) {
    int getInt;
    printf("%s", text);
    scanf("%i", &getInt);
    return getInt;
}
```

위의 코드에서 변경해 줄 수 있는 부분이 있다.
```c
int days = age * 365;
printf("Your are at least %i days old\n", days);
=>
printf("Your are at least %i days old\n", age * 365); 
```
days 라는 변수를 사용하지않고 직접 계산하는 수식을 넣어주면 코드 한줄을 줄일 수 있다.
  
여기서 한줄을 더 줄일 수 있는데
```c
int age = get_int("What's your age?\n");
printf("Your are at least %i days old\n", age * 365); 
=>
printf("Your are at least %i days old\n", get_int("What's your age?\n")); 
```
이렇게하면 한줄의 코드로 표현이 가능하다.  

그러나 이 표현은 한줄로 간결하게 사용가능하지만 가독성이 나쁘고 한눈에 코드를 알아보기가 쉽지않다.
작성한 코드가 틀렸다 맞다의 개념보다는 코드의 디자인이고 방향성이다. 어떤걸 선택하고 어떤걸 포기할지 그리고 현재 주어진 상황에서 어떤것이 가장 최선의 선택이고 방법일지에 대한 고민을 해봐야할 필요성이 있다.
  

이후 예제에서는 실수와 다른 표현예제를 설명하면서 get_float 등의 함수들을 사용하는데 이후 강의에서 int 와 string 을 제외한 함수는 사용 빈도가 많지않아 따로 함수를 만들지는 않았다.
  

**소수점의 길이를 표현하는 방법**
```c
printf("%.2f", 1.0651111); // .2 로 소수점이하 둘째자리까지만 표현가능하다.
```


**사용자에게서 입력받은 숫자를 홀수, 짝수 구분하는 코드**
```c
int main(void)
{
    int n = get_int("n: ");

    if (n % 2 == 0) // n 을 2로 나눈 나머지가 0이면
    {
        printf("even\n");
    }
    else 
    {
        printf("odd\n");
    }
}
```
숫자의 짝/홀 구분은 딱 두가지 경우의 수만 존재한다. 홀수 아니면 짝수 따라서 else if 로 새로운 조건을 작성하지않아도 else 만으로 첫번째 조건 이후의 나머지 조건에 대해서 어떻게 출력할지를 정할 수 있다.
  
예제 코드에 대한 설명을 붙이면서 `// 내용작성` 이런걸 많이 사용했는데 이건 주석이라고 부르고 코드실행에서는 표현되지않는 노트작성 기능이라고 볼 수 있다.  

해당 표현은 한줄만 주석처리가 가능하고 mac 에서는 `command + /`,  windows 에서는 `ctrl + /` 가 단축키이다. 만약 여러줄의 주석을 사용하고싶다면 `/* 주석내용 */` 으로 여러줄의 주석을 작성 할 수 있다.

### 여러개의 조건문 사용
```c
if (x < y)
{

}
```
위와 같은 형태의 한개의 조건식만 들어갈수도 있지만 or / and 연산자를 사용하여 여러개의 조건을 한줄에 추가해 줄 수도있다.

```c
if (x < 50 || y > 10) // or 연산자
{
 // 둘중 하나만 만족하면 여기 실행
}
```
위 코드는 두 조건중에 단 하나만 만족하면 코드가 실행이 된다.
```c
if (x < 50 && y > 10) // and 연산자
{
 // 두 조건 모두 만족해야 여기 실행
}
```
위 코드는 두 조건이 모두 만족해야만 코드가 실행이 된다.



## 사용자 정의 함수, 중첩 루프

**cough 를 세번 출력하는 방법 01**
```c
// 하드코딩
#include <stiod.h>

int main(void)
{
    printf("cough\n");
    printf("cough\n");
    printf("cough\n");
}
```

**cough 를 세번 출력하는 방법 02**
```c
// 루프를 이용한 실행
#include <stiod.h>

int main(void)
{
    for(int i = 0; i < 3; i++) {
        printf("cough\n");
    }
}
```

**cough 를 세번 출력하는 방법 03**
```c
// 사용자 함수로 실행
#include <stiod.h>

void cough(void); // main 아래에 구현된 함수를 위로 끌러올려 main 에서 사용 가능하도록 함수를 선언

int main(void) // main 함수가 상단에 있을수록 코드 가독성이 좋다.
{
    for(int i = 0; i < 3; i++) {
        cough();
    }
}

// 사용자 함수
void cough(void) // void 는 리턴값이 없는 함수를 선언 (void) 는 받아오는 인자값이 없다라는것
{
    printf("cough\n");
}

/*
result

cough
cough
cough
*/
```

**cough 를 세번 출력하는 방법 04**
```c
// 사용자 입력값에 따른 반복 횟수를 정의
#include <stiod.h>

void cough(int n);

int main(void)
{
    int num = get_int("input number\n");
    cough(num);
}

// 사용자 함수
void cough(int n)
{
    for(int i = 0; i < n; i++) {
        printf("cough\n");
    }
}
```

**값을 리턴하는 사용자 함수**
```c
#include <stiod.h>

int get_positive_int(void);

int main(void)
{
    int result = get_positive_int();
    printg("%i", result);
}

int get_positive_int(void) // 정수형 타입을 리턴하는 함수
{
    int n; // n 이라는 변수명 선언
    do // do - while 문은 조건에 상관없이 무조건 최초 1번은 실행되는 코드이다 
    {
        n = get_int("Positive Integer: ");
    }
    while (n < 1);
    return n;
}
```

### 중첩루프
동일한 모양의 벽돌을 쌓는다고 했을때 이걸 코드로 표현하려면 중첩루프를 사용 할 수 있다.   
벽돌은 일정한 갯수대로 깔리고 일정한 갯수만큼 쌓이게 된다. 내부 루프는 왼쪽에서 오른쪽으로 쌓이는 블록을 표현 할 수 있고 외부 루프는 이 블록의 층을 쌓는 것을 표현 할 수 있다.

```c
#include <stdio.h>

int main(void)
{
    int n = 3;
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            printf("#");
        }
        printf("\n");
    }
}

/*
result
###
###
###
*/
```

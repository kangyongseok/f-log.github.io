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

컴파일되는 과정은 크게 네가지로 분류할 수 있다.

- 전처리
- 컴파일링
- 어셈블링
- 링킹

### 전처리(Precompile)
코드상에서 가장 맨위에있는 `#include <stdio.h>` 와 같은 형태의 코드들을 현재 코드에서 사용되어지고있는 함수들만 가져와 코드에 포함시키는 역할을 한다.

### 컴파일링
그럼 현재 코드에는 완전히 까여진 코드들만 존재하게 될것이다. 그럼 이 코드들을 컴퓨터가 이해하기에 쉬운 좀더 저수준 언어에 가까운 어셈블리어로 변환하는과정을 거치게된다.

### 어셈블링
그리고 이곳에서 어셈블리어로 변경된 코드를 기계가 이해가능한 0 과 1형태로 변경하는 작업이 진행된다.

### 링킹
마지막으로 여러곳에 따로 작성되어져있던 코드들을 하나의 파일로 합치는 작업이 진행된다.

## 배열

여러가지 자료형  
- bool 1byte - (예) True, False, 1, 0, yes, no
- char(%c) 1byte - 문자 하나 (예) 'a', 'Z', '?'
- int(%i) 4byte - 특정 크기 또는 특정 비트까지의 정수 (예) 5, 28, -3, 0
- float(%f) 4byte - 부동소수점을 갖는 실수 (예) 3.14, 0.0, -28.56
- long(%li) 8byte - 더 큰 크기의 정수
- double 8byte - 부동소수점을 포함한 더 큰 실수

```c
#include <stdio.h>

float average(int length, int array[]);

int main(void) {
    // before
    // int score1 = 71;
    // int score2 = 72;
    // int score3 = 33;
    // printf("Average : %i\n", (score1 + score2 + score3) / 3);

    // after
    int n;
    printf("Number of scores: ");
    scanf("%i", &n);

    int scores[n];
    for(int i = 0; i < n; i++) {
        // scores[i];
        printf("Score %i: ", i + 1);
        scanf("%i", &scores[i]);
    }
    printf("Average: %.1f\n", average(n, scores));
}

float average(int length, int array[]) {
    int sum = 0;
    for (int i = 0; i < length; i++) {
        sum += array[i];
    }
    return (float) sum / (float) length;
}
```

여기서 score1 에 72 라는 값은 int 타입이기때문에 4byte의 메모리영역을 차지하게 된다.  
나머지 score2, 3 도 각각 4byte 의 메모리 영역을 차지한다.


## 문자열
문자열을 가진 변수들은 각각 메모리에 저장되는데 문자열같은경우 문자열이 끝났다 라는것을 구분하기위해 문자열 마지막에 항상 null 을 나타내는 0 이 포함된다.
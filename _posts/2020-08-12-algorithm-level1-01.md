---
title: "알고리즘 - 2016년_Lv1"
categories: 
  -  Algorithm
tags: 
    - Algorithm
    - coding
    - c
    - 프로그래머스
    - 알고리즘풀이
    - 연습
toc: true
toc_sticky: true
comments:  true
---

## 문제
`2016년 1월 1일은 금요일`입니다. `2016년 a월 b일은 무슨 요일`일까요? 두 수 a ,b를 입력받아 2016년 a월 b일이 무슨 요일인지 리턴하는 함수, solution을 완성하세요. 요일의 이름은 일요일부터 토요일까지 각각 `SUN,MON,TUE,WED,THU,FRI,SAT` 입니다.   
예를 들어 a=5, b=24라면 5월 24일은 화요일이므로 문자열 TUE를 반환하세요.

## 조건
2016년은 윤년(2월 29일)입니다.  
2016년 a월 b일은 실제로 있는 날입니다. (13월 26일이나 2월 45일같은 날짜는 주어지지 않습니다)

## 입출력 예

### test case 01
```c
int a = 5
int b = 24
char result = "THU"
```

### test case 02
```c
int a = 1
int b = 3
char result = "SUN"
```

## 풀이
```c
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>

char* solution(int a, int b) {
    // 리턴할 값은 메모리를 동적 할당해주세요.
    char *days[] = {"SUN","MON","TUE","WED","THU","FRI","SAT"};
    char *month[12] = {31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    char* answer = (char*)malloc(sizeof(days) / sizeof(char));
    int totalDay = 0; // 2월이면 1월의 전체일수 + b
    int test = 0;
    for (int i = 0; i < a - 1; i++) {
        totalDay += month[i];
    }
    totalDay += b;
    printf("%d", totalDay);
    if ((totalDay % 7) + 5 >= 7) {
        test = ((totalDay % 7) + 4) % 7;
    } else {
        test = ((totalDay % 7) + 4);
    }
    return answer = days[test];
    free(answer);
}
```

해당 문제는 전체 일수에서 7로 나눈 나머지 값이 month 의 index 로 들어가 값을 출력하게되는 형태이다.  

전체일수를 7로 나눈 나머지값이 month의 index 로 들어가 요일을 출력하게되는데 요일 배열이 일요일부터 0으로 시작하고 기준일인 1월1일은 금요일로 배열상에서 5번째에 위치해 있기때문에 +5일을 더해야 했지만 배열은 0부터 6까지이고 나눈 일수는 7일로 일주일이기때문에 이 차이를 계산해서 -1일 한 +4일을 더해줘서 금요일부터 시작한 값을 더해주었다.  

원래대로 7로 나누면 나머지는 7보다 작은 숫자가 나와야 하지만 금요일부터 시작하는만큼 +4를 해주었기 때문에 7을 넘어버려 index 값이 없는값이 나와버린다. 때문에 나머지로 나온값이 7보다 크거나 같다면 7로 한번더 나눠주는 수식을 붙였다.
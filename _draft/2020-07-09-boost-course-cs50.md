---
title: "부스트코스 - 모두를 위한 컴퓨터 과학 (CS50 2019) - 알고리즘"
categories: 
  -  boostcourse
tags: 
    - develop
    - study
    - edwith
    - boostcourse
    - cs50
    - C
    - 알고리즘
toc: true
toc_sticky: true
comments:  true
---

## 알고리즘
### 선형탐색
처음부터 끝까지 하나하나 일일이 확인하면서 탐색하는 방법

### 이진탐색
전화번호부에서 절반씩 나눠가면서 탐색해 나가는 방법

### Big-O 표기법
O(n) or O(n/2) or O(log2n) 이라고 표기함  
그러나 일반적으로 표현하는 방법은 `O(n)`과 `O(logn)` 이라고 표기한다. 이유는 더 많은 값들을 그래프 안에 표현하게된다면 n 과 n/2 의 그래프는 한없이 가까워질것이기때문에 의미가 없고 logn 또한 숫자가 몇이붙던 비슷한 그래프를 형성하기 때문에 저 두 형태로만 표기한다


### Omega
omega 는 효율성을 나타냄

### 전화번호부를 찾는 코드작성
```c
int main(void)
{
    string names[4] = {"EMMA", "YONG", "KIM", "HONG"};
    string numbers[4] = {"010-1234-1234", "010-2222-1234", "010-3333-1234", "010-4343-1234"};

    for (int i = 0; i < 4; i++)
    {
        if (strcmp(names[i], "EMMA") == 0)
        {
            printf("%s\n", numbers[i]);
            return -;
        }
    }
    printf("Not Found\n");
    return 1;
}
```
위 코드의 문제점은 이름과 번호 정보를 가진게 현재 1:1매칭이 정렬이 된상태로 되어있는데 만약 데이터가 더 늘어나고 정렬의 기준이 바뀐다면 더이상 이름과 번호는 서로 매칭이 되지않게된다.

### 개선된 코드 작성

```c

typedef structure
{
    string name;
    string number;
}
person;

int main(void)
{
    person people[4];
    
    people[0].name = "EMMA";
    people[0].number = "010-1234-1234";
    
    people[1].name = "KANG";
    people[1].number = "010-1234-1333";
    
    people[2].name = "KIM";
    people[2].number = "010-1234-1222";
    
    people[3].name = "HONG";
    people[3].number = "010-1234-1111";

    for (int i = 0; i < 4; i++)
    {
        if (strcmp(people[i].name, "EMMA") == 0)
        {
            printf("%s\n", people[i].number);
            return -;
        }
    }
    printf("Not Found\n");
    return 1;
}
```
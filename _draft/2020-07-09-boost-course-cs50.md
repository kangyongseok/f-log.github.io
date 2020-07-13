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


## 버블정렬
정렬되지 않은 리스트를 탐색하는것보다는 정렬되어있는 리스트를 탐색하는것이 더 빠르다. 이러한 정렬을 위해서 사용하는 알고리즘중에 버블정렬이 있다.  

버블정렬은 두개의 인접한 자료값을 비교하면서 위치를 교환하는 방식으로 정렬한다. 단 두개의 요소만 비교해서 정렬을 해 나가기 때문에 매우 오랜시간이 걸릴 수 있다. 그리고 버블정렬은 정렬이 된 리스트를 적용해도 정렬의 여부와 상관없이 무조건 비교를 진행하게된다. 단지 정렬의 위치가 전혀 바뀌지 않을 뿐이다.  

즉 n개의 요소를 정렬할때 최소한 n-1번의 실행이 필요하다.

```console
1회차 [5, 1, 6, 2, 4, 3] 5와 1을 비교
2회차 [1, 5, 6, 2, 4, 3] 5와 6을 비교
3회차 [1, 5, 6, 2, 4, 3] 6과 2를 비교
4회차 [1, 5, 2, 6, 4, 3] 6과 4를 비교
5회차 [1, 5, 2, 4, 6, 3] 6과 3를 비교
6회차 [1, 5, 2, 4, 3, 6] 5와 2를 비교
7회차 [1, 2, 5, 4, 3, 6] 5와 4를 비교
8회차 [1, 2, 4, 5, 3, 6] 5와 3를 비교
.
.
.
.

```

## 선택정렬
무작위로 정렬된 숫자가 있을때 숫자 배열에서 가장 작은값을 찾아 가장 왼쪽부터 정렬해 나가는 방법이다. 반복할떄마다 이미 정렬된 값은 제외하고 가장 작은 값을 찾아 첫번째 행위를 반복한다.

```c
// 의사코드
// 0 = 가장 왼쪽
// n-1 배열의 가장 마지막
for i from 0 to n - 1
    i 번째부터 가장 마지막 항목중에 가장 작은 값을 찾는다.
    가장 작은 값을 찾았다면 i 번쨰 항목과 교체한다.
```
n + (n - 1) + (n - 2)+ ..... + 1  
n(n + 1)/2  

버블정렬과 전혀 다른 알고리즘 방식을 사용하지만 결과적으로는 가장 베스트의 정렬이 놓여있다 하더라도 컴퓨터는 모든 배열을 다 열어봐야 확인이 가능하기때문에 이것은 버블정렬과 동일하고 Big-O 과 Omega 로 나타냈을때는 똑같은 결과가 나오게 된다.

## 더 나은 방법
```c
// 버블정렬
Repeat n-1 times
    For i from 0 to n-2
        if i > i+1 
            Swap them

/// 개선
Repeat until no Swaps
    For i from 0 to n-2
        if i > i+1 
            Swap them
```

## 재귀
```c
// 기존 의사코드
Pick up phone book
Open to middel of phone book
Look at page // line 3
If Smith is on page
    Call Mike
Else if Smith is earlier in book
    Open to middle of left half of book
    Go back to Line 3
Else if Smith is later in book
    Open to middel of right half of book
    Go back to line 3
Else
    Quit
```

```c
// 기존 의사코드
Pick up phone book
Open to middel of phone book
Look at page
If Smith is on page
    Call Mike
Else if Smith is earlier in book
   Search left half of book // 추가
Else if Smith is later in book
   Search 갸홋 half of book // 추가
Else
    Quit
```

재귀함수는 자기 자신을 호출 여기서는 Search 함수를 반복적으로 호출하여 행위의 반복을 하면서 코드의 양을 줄일 수 있다.

## 병합정렬 (or 합병정렬)

배열의 원소가 한개가 될때까지 계속해서 반으로 나누다가 왼쪽과 오른쪽 절반씩을 합쳐나가면서 정렬하는 방식, 재귀적으로 동작하기 떄문에 재귀에 대한 이해가 필요하다.

```c
// 의사코드
if only one item
    Return
Else
    Sort left half of items
    Sort right half of items
    Merge sorted halves
```
[7, 4, 5, 2, 6, 3, 8, 1]  

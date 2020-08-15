---
title: "부스트코스 - 모두를 위한 컴퓨터 과학 (CS50 2019) - Mission"
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

## ✔︎문제 1

해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.

## Solution
```c
#include <stdio.h>

int main(void)
{
    int orderCount = 3;
    int currentCount = 5;
    int restCount = 0;
    int totalPrice = 0;
    int cost = 10000;
    float vat = 0.1;

    restCount = currentCount - orderCount;
    totalPrice = orderCount * cost;


    printf("주문건수 : %d 건\n", orderCount);
    printf("기존 재고량 : %d개\n", currentCount);
    printf("남은 재고량 : %d개\n", restCount);
    printf("매출액(부가세포함) : %.0f원\n", (totalPrice * vat) + totalPrice);
}
```

 

 ## ✔︎ 문제 2

 해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.

## Solution
 ```c
#include <stdio.h>

int main(void)
{
    int goalMoney;

    printf("목표금액을 입력하세요: ");
    scanf("%d", &goalMoney);

    printf("%.0f", (goalMoney * 0.012) + goalMoney);
}
 ```

 

 

## ✔︎ 문제 3
해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.

## Solution

```c
#include <stdio.h>
#include <string.h>

int main(void) {
    char str[10];
	int length;
    char *week[] = {"월요일", "화요일", "수요일", "목요일", "금요일", "토요일", "일요일"};
	char *food[] = {"청국장", "된장찌개", "두부김치", "오삼불고기", "소불고기", "미트볼", "김치찌개"};
	
	int arrNum = sizeof(week) / sizeof(week[0]);
    
    while(1) {
        printf("요일을 입력하세요: ");
        scanf("%9s", str); 
        for(int i = 0; i < arrNum; i++) {
            if (strcmp(str, week[i]) == 0) {
                printf("%s:%s\n", week[i], food[i]);
                return 0;
            }
        }
    }
    return 0;
}
```
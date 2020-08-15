---
title: "부스트코스 - 모두를 위한 컴퓨터 과학 (CS50 2019) - 3주차 Mission"
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
    - 배열
    - queue
toc: true
toc_sticky: true
comments:  true
---

## ✔︎ 문제 1. 학점을 계산해보자!


### 1. 미션 제목
해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h> // isdigit 함수사용
#include <stdlib.h> // atoi 함수 사용

const int NUMBER_OF_GRADES = 9;
// printGradeTable() 함수를 사용하기 위하여 타입을 char 형태로 통일하기 위해 점수도 스트링 타입으로 입력
const char *GRADE_POINTS[NUMBER_OF_GRADES] = {"95", "90", "85", "80", "75", "70", "65", "60", "0"};
const char *GRADES[NUMBER_OF_GRADES] = {"A+", "A", "B+", "B", "C+", "C", "D+", "D", "F"};

void printGradeTable(char *label, const char *target[]);
int checkNum(char *str);
int resultGradePoint(int numGrade, int length);

int main(void) {
    char strGrade[4];
    printf("학점프로그램\n");
    printf("종료를 원하면 \"999\" 를 입력\n");
    printf("[학점 테이블]\n");
    printGradeTable("점수", GRADE_POINTS);
    printf("\n");
    printGradeTable("학점", GRADES);
    printf("\n");

    int arrNum = sizeof(GRADE_POINTS) / sizeof(GRADE_POINTS[0]);

    while(1) {
        int numGrade;
        printf("성적을 입력하세요 (0 ~ 100) : ");
        scanf("%s", strGrade);
        if (checkNum(strGrade) == -1) {
            printf("숫자가 아닙니다 프로그램을 종료합니다.\n");
            break;
        }

        numGrade = atoi(strGrade); // 문자 정수를 숫자로 변환

        if (numGrade == 999) {
            printf("학점 프로그램을 종료합니다.\n");
            break;
        }
        if(numGrade > 100 || numGrade < 0) {
            printf("** %d 성적을 올바르게 입력하세요! (0 ~ 100)\n", numGrade);
            continue;
        }

        resultGradePoint(numGrade, arrNum);
    }
}

void printGradeTable(char *label, const char *target[]) {
    // 학점 테이블 출력 함수
    printf("%s : ", label);
    for (int i = 0; i < NUMBER_OF_GRADES; i++) {
        printf("%s\t", target[i]);
    }
}

int checkNum(char *str) {
    int const length = strlen(str);
    int result;
    // 문자 정수를 문자배열로 취급하여 숫자인지 아닌지 하나씩 체크 하나라도 숫자가 아니면 -1
    // ex) str = 20 => ["2", "0"] => isdigit() 함수로 문자가 숫자 문자인지 체크(연속된 문자가 아닌 한글자씩만 체크가능)
    for (int i = 0; i < length; i++) {
        if (isdigit(str[i]) == 0) {
            // 숫자가 아님
            result = -1;
            break;
        }
        result = 0;
    }
    return result;
}

int resultGradePoint(int numGrade, int length) {
    for (int j = 0; j < length; j++) {
        if (numGrade == 0) {
            printf("학점은 F 입니다.\n");
            break;
        }
        if (numGrade >= 95) {
            printf("학점은 A+ 입니다.\n");
            break;
        }
        if (atoi(GRADE_POINTS[j]) >= numGrade && atoi(GRADE_POINTS[j + 1]) <= numGrade) {
            printf("학점은 %s 입니다.\n", GRADES[j + 1]);
            break;
        }
    }
    return -1;
}
```



## ✔︎ 문제 2. 누가 빠졌는지 찾아보자!
해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.
 
```c
#include <stdio.h>
#define SIZE 500000

int main(int arge, char*argv[]) {
    int n;

    scanf("%d", &n);

    // 1부터 N의 숫자중 K가 빠진 배열
    int partArr[SIZE];
    int lengthOfPartArr = n-1;

    for(int i=0; i < lengthOfPartArr; i++){
        scanf("%d", &partArr[i]);
    }

    // TODO: n과 partArr를 이용하여, K를 구하라
    return 0;
}
```

풀이
```c
#include <stdio.h>
#define SIZE 500000

int main(int arge, char*argv[]) {
    int n;
    int nSum;
    int sum;

    // scanf 는 \n 즉 엔터가 나오기 전까지있는 숫자와 문자를 출력한다 해당 파일들은 각각 10, 100... 등의 숫자가 있고 바로 엔터가 있기때문에 n에는 자동으로 각 파일의 엔터전에있는 첫번째 숫자값이 자동으로 들어온다.
    scanf("%d", &n);

    // 1부터 N의 숫자중 K가 빠진 배열
    int partArr[SIZE]; // SIZE 만큼의 메모리 영역을 할당
    int lengthOfPartArr = n-1; // 받아온 각 첫번째 숫자보다 -1 된 숫자를 저장

    // 1 부터 N 까지의 정상적인 숫자의 합
    for (int j = 0; j <= n; j++) {
        nSum += j;
    }

    // 1 부터 N-1 까지의 숫자의 합
    for(int i=0; i < lengthOfPartArr; i++){
        // scanf 는 엔터를 만나기전까지의 데이터를 리턴하지만 띄어쓰기가 있어도 똑같이 동작함
        // 띄어쓰기를 만날때마다 partArr[i] 배열에 값을 하나씩 저장
        scanf("%d", &partArr[i]);
        sum += partArr[i];
    }

    // 1 부터 N 까지의 숫자를 더한 총 값에서 N-1 의 숫자를 더한값을 빼면 빠진값을 구할 수 있음
    printf("K = %d", nSum - sum);


    // TODO: n과 partArr를 이용하여, K를 구하라
    return 0;
}
```

### 결과
![결과](https://cphinf.pstatic.net/mooc/20200724_162/1595569303774wktme_PNG/mceclip0.png)

## ✔︎ 문제 3. Queue를 만들어보자!

해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.

 ```c
#include <stdio.h>
#include <string.h>
#include <ctype.h> 

#define MAX 10
#define TRUE 1
#define FALSE 0

int front = 0;
int rear = 0;
int queueArr[MAX];
int loopEnd = FALSE;

void enqueue(void);
void dequeue(void);
void quit(void);
void display(void);
int get_int(char *text);

int main(void) {
    while(!loopEnd) {
        int option = get_int("옵션번호를 입력하세요(1. enqueue, 2. dequeue, 3. display, 4. quit) : ");
        // getchar 는 stdin에있는 문자를 읽어들이는데 그러면 scanf 입력버퍼에 남아있던 \n 을 가져옴 != \n 처리를 한 이유는 그냥 getchar() 함수를 사용하면 띄어쓰기 이후에 남아있던 다른 문자까지 모두 가져와 입력버퍼에서 제거해버리기 때문에 필요한 비교구문
        while (getchar() != '\n');
        switch(option) {
            case 1:
                enqueue();
                break;
            case 2:
                dequeue();
                break;
            case 3:
                display();
                break;
            case 4:
                quit();
                break;
            default:
                break;
        }
    }
}

void enqueue(void) {
    if ((rear + 1) % MAX == front) {
        printf("Queue가 꽉 찼습니다.\n");
    } else {
        int input_value = get_int("값을 입력해주세요 : ");
        rear = (rear + 1) % MAX;
        queueArr[rear] = input_value;
    }
}

void dequeue(void) {
    if (front == rear) {
        printf("Queue 가 비었습니다.\n");
    } else {
        front = (front + 1) % MAX;
        printf("%i\n", queueArr[front]);
    }
}

void display(void) {
    printf("==============================================================\n");
    printf("Queue에 남은값 : ");
    for(int i = front + 1; i < rear + 1; i++) {
        printf("%i ", queueArr[i]);
    }
    printf("\n");
    printf("==============================================================\n");
}

void quit(void) {
    printf("************************************************************\n");
    printf("Queue 프로그램 종료\n");
    printf("************************************************************\n");
    loopEnd = TRUE;
}



int get_int(char *text) {
    int getInt;
    printf("%s", text);
    scanf("%i", &getInt);
    return getInt;
}
 ```
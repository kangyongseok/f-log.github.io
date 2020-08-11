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
    학점을 계산해보자!

 

### 2. 지시문
    - 학생의 점수로 학점을 구하는 프로그램을 작성하시오.
    - 키보드에서 입력받은 성적 (0 ~ 100 점) 의 유효성을 체크
    - 학점은 배열을 이용하여 초기화 (아래 “학점 테이블” 참조)
    - 학점은 “학점” 과 같이 계산하는데, 반드시 “학점 테이블”을 사용하여 계산하고 학점도 “학점 테이블”의 내용을 출력
    - 성적을 입력하여 계속 학점을 구하며 특별한 문자인 “-1” 을 입력하면 프로그램을 종료

 

Table 1 - 학점
![table](https://cphinf.pstatic.net/mooc/20200724_16/1595567085050XPu8n_PNG/mceclip0.png)

Table 2 - 학점 테이블
![table2](https://cphinf.pstatic.net/mooc/20200724_14/1595567195243bMUTK_PNG/mceclip1.png)

 

유효성 체크: 0 <= 성적 <= 100
    - “120” 입력 -> 성적을 올바르게 입력하세요! (0 ~ 100)

Figure 1 출력 결과
![result](https://cphinf.pstatic.net/mooc/20200724_68/1595567313265FDFs9_PNG/mceclip2.png)

### 3. 핵심 개념
#배열 #표준입력 #표준출력 #분기문 #반복문 #break #무한반복문


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

 

### 1. 미션 제목
누가 빠졌는지 찾아보자!

 

### 2. 지시문
1 부터 N 까지의 숫자 모음이 있다. 여기서 K 라는 숫자가 빠진 N – 1 개의 파일이 있다. K 라는 숫자를 찾아보자.  
    - N 은 2 이상 500,000 이하의 값을 가짐  
    - 정렬되지 않은 숫자들의 모음 파일이 주어짐 (ex, 10.txt 100.txt 1_000.txt 10_000.txt 100_000.txt, 500_000.txt)  
    - 위 파일에서 빠진 숫자 K 를 찾아라  
    - 파일의 값을 읽어 n 과 k 가 빠진 arr 이를 저장하는 참고 코드는 아래 참고 (파일은 제공된 파일 사용)  
    - 파일 구조: 첫 줄에는 N 값이 주어지고, 그 아래 줄에는 공백으로 K 를 제외한 N – 1 개의 숫자들이 나열 됨  

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

 

### 1. 미션 제목
Queue 를 만들어 보자!

 

### 2. 지시문
배열을 이용하여 Queue 를 만들어 보자.  
특정 업무를 할 때, 우리는 일을 들어온 순서대로 해야할 때가 있다. 은행 업무를 예를 들어보자. 은행업무를 보기 위한 고객들이 10명이 있다고 치고, 각자 대기표가 있다. 그럼 은행원들은 각자 업무가 끝나면 대기한 고객을 순서대로 뽑아야 할 것이다. 이때 필요한 것이 Queue 이다. (1) 대기표를 뽑는다 (Queue 에 데이터를 삽입). (2) 대기인원을 보여준다 (queue 에 쌓여있는 데이터 조회). (3) 순서대로 대기인원을 호출한다 (queue 를 하나씩 pop 한다).  


- Queue 자료구조를 array를 이용해 구현
1. add (1), pop (2), display (3), quit (4) 기능 구현
2. 입력 한 옵션 (1, 2, 3, 4) 에 따라 switch 문을 사용하여 각각의 기능을 수행하도록 구현
3. 필요한 함수 목록: insert(), delete(), display()
    - 각 함수의 파라미터는 필요하면 정의하기
4. add() 함수의 정의
    - queue 가 꽉찼는지 확인 (Queue 의 max 크기는 10으로 정의), queue 가 꽉찼으면 “Queue가 꽉 찼습니다.” 를 출력
    - queue 에 삽입이 가능하면, 값을 입력 받아 queue 배열에 삽입 (hint: front, rear 변수를 사용하여 queue 의 현재 위치를 저장한다)
5. pop() 함수의 정의
    - queue 가 비었는지 확인, 비었으면 “Queue가 비었습니다.” 를 출력
    - queue 가 비어있지 않으면, 가장 먼저 들어온 순서로 값을 하나 가져와 출력 (hint: front 변수값 조정 필요)
6. display() 함수의 정의
    - 반복문을 사용하여 배열의 모든 요소를 출력 (hint: front, rear 변수 범위로 배열값을 출력)

 

TODO: 여기서 출력 예시 보여주기

 

### 3. 핵심 개념 (키워드 제시)
#배열 #Queue #switch문 #반복문 #표준입출력

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
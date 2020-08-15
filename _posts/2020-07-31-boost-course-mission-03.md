---
title: "부스트코스 - 모두를 위한 컴퓨터 과학 (CS50 2019) - 4주차 Mission"
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

## ✔︎ 미션 1.


### 1. 미션 제목
해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.
 ```c
#include <stdio.h>
#define MAX 5
#define TRUE 1
#define FALSE 0

void selectSort(int *arr);
void bubbleSort(int *arr);
void mergeSort(int *arr, int start, int end);
void swap(int x, int y, int tmp, int *arr);
void compare(int *x, int *y);
void merge(int *arr, int start, int mid, int end);

int selectSort_1[MAX] = {1, 2, 3, 4, 5};
int selectSort_2[MAX] = {5, 4, 3, 0, 1};
int bubbleSort_1[MAX] = {1, 4, 2, 5, 8};
int bubbleSort_2[MAX] = {2, 5, 4, 3, 1};
int recursive_1[MAX] = {1, 1, 1, 3, 2};
int recursive_2[MAX] = {2, 1, 1, 3, 1};
int mergeSortResult[MAX]; // 병합정렬된 배열이 들어갈 변수

int main(void) {
    selectSort(selectSort_1);
    selectSort(selectSort_2);
    bubbleSort(bubbleSort_1);
    bubbleSort(bubbleSort_2);
    mergeSort(recursive_1, 0, MAX - 1); // 배열데이터와함께 시작 과 끝 인텍스값을 넘기기때문에 MAX - 1 을 넘겨줍니다.
    mergeSort(recursive_2, 0, MAX - 1);

    compare(selectSort_1, selectSort_2);
    compare(bubbleSort_1, bubbleSort_2);
    compare(recursive_1, recursive_2);

    return 0;
}

void selectSort(int *arr) {
    // 선택정렬
    // n개의 배열중 가장 작은값을 찾아 0부터 n-1 번째까지 반복문 돌면서 0번 인덱스부터 스왑
    for (int i = 0; i < MAX - 1; i++) {
        int minIndex = i;
        int tmp;
        for (int j = i + 1; j < MAX; j++) {
            if (arr[minIndex] > arr[j]) {
                minIndex = j;
            }
        }
        if (i != minIndex) { // 자기 자신이 최소값일때 swap 하지않음
            swap(i, minIndex, tmp, arr);
        }
    }
}

void bubbleSort(int *arr) {
    // 버블정렬
    // n 개의 배열을 돌면서 n 과 n + 1 의 값을 비교하여 n + 1 이 더 작다면 swap
    // 정렬이 진행될때마다 제일 오른쪽 배열부터는 더이상 비교할 필요가 없어 그만큼 비교횟수를 -1 해줘야한다.
    int tmp;
    for(int i = 0; i < MAX -1; i ++) {
        for (int j = 0; j < MAX - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(j, j + 1, tmp, arr);
            }
        }
    }
}

void mergeSort(int *arr, int start, int end) {
    // 재귀정렬
    if (start < end) {
        int mid = (start + end) / 2;
        // printf("mid: %d\n", mid);
        mergeSort(arr, start, mid); // 배열의 처음부터 중간까지
        mergeSort(arr, mid + 1, end); // 배열의 중간 다음배열부터 마지막 배열까지
        merge(arr, start, mid, end); // mergeSort 의 실행이 모두 끝나야 merge 함수실행
    }
}

// 기타 사용 함수
void swap(int x, int y, int tmp, int *arr) {
    tmp = arr[x];
    arr[x] = arr[y];
    arr[y] = tmp;
}

void compare(int *x, int *y) {
    int result;
    for (int i = 0; i < MAX; i++) {
        if (x[i] != y[i]) {
            result = FALSE;
            break;
        } else {
            result = TRUE;
        }
    }
    printf("입력값: %d%d%d%d%d, %d%d%d%d%d ", x[0], x[1], x[2], x[3], x[4], y[0], y[1], y[2], y[3], y[4]);
    printf("출력값: %s\n", result == 0 ? "False" : "True");
}

void merge(int *arr, int start, int mid, int end) {
    int i = start;
    int j = mid + 1;
    int k = start; // 정렬된 배열을 담을 변수의 index
    // printf("mid :%d start: %d end: %d\n", mid, start, end);
    while(i <= mid && j <= end) { 
        if (arr[i] <= arr[j]) { // 앞의 값보다 뒤의값이 크거나 같은지
            mergeSortResult[k] = arr[i];
            i++;
        } else { // 뒤에비교하는 숫자가 더 작을경우
            mergeSortResult[k] = arr[j];
            j++;
        }
        k++;
    }

    if (i > mid) { // while 문에서 배열에 할당하지 못한 나머지 값을 여기서 할당한다.
        for(int t = j; t <= end; t++) {
            mergeSortResult[k] = arr[t];
            k++;
        }
    } else {
        for(int t = i; t <= mid; t++) {
            mergeSortResult[k] = arr[t];
            k++;
        }
    }
    for (int t = start; t <= end; t++) { // 정렬된 배열을 원래 배열에 넣는다.
        arr[t] = mergeSortResult[t];
    }
}
 ```

   
## ✔︎ 미션 2.
해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
// #include <math.h>
#define TRUE 1
#define FALSE 0

// 요구사항 자체가 거리를 예시로들면서 정해진 순서대로 숫자가 입력된다고 보았습니다.
// 요구사항에는 없는 정렬코드가 들어가게되면 불필요한 로직이 생기고 불필요한 실행시간이 소요되기때문에 순수하게 거리의 센터위치를 구현하는데 중점을 두었습니다.
// 정렬 관련된 문제가 아닌것같아서 저는 모든 값은 이미 정렬된 순서대로 들어온다고 가정하고 코드를 작성하였습니다.
// 입력된 배열 내에서 센터에 가장 근접한 값을 찾아 리턴하는 문제라고 이해하고 접근하였습니다.
int inputData_1[4] = {0, 4, 6, 9};
int inputData_2[4] = {2, 2, 2, 4};
int inputData_3[6] = {1, 2, 3, 5, 7, 9};
int inputData_4[6] = {1, 2, 10, 23, 31, 35};

// void getIntArrSize(int *arr);
void findCenterHouse(int arr[], int count);
int averageDistance(int arr[], int count);

int main(void) {
    clock_t start, end;
    double result;
    start = clock(); //시간 측정 시작
    findCenterHouse(inputData_1, sizeof(inputData_1) / sizeof(int));
    findCenterHouse(inputData_2, sizeof(inputData_2) / sizeof(int));
    findCenterHouse(inputData_3, sizeof(inputData_3) / sizeof(int));
    findCenterHouse(inputData_4, sizeof(inputData_4) / sizeof(int));

    end = clock(); //시간 측정 끝
    result = (double)(end - start);
    printf("실행시간: %.0f\n", result);

    return 0;
}

void findCenterHouse(int arr[], int count) {
    int average = averageDistance(arr, count);
    int isCenterHouse = FALSE;
    for (int i = 0; i < count; i++) { // 평균 지점과 일치하는 값이 배열에 있을때
        if(arr[i] == average) {
            isCenterHouse = average;
            break;
        }
    }
    if (isCenterHouse == FALSE) { // 평균 지점과 일치하는 센터값이 배열에 없을때
        int maxOfSmallValue; // 평균값보다 작은값중에 가장 큰값
        int minOfBigValue; // 평균값보다 큰값중에 가장 작은값

        for(int i = 0; i < count; i++) { // 센터값과 가장 근사치에있는 양쪽의 값을 구함
            if (average > arr[i]) {
                // printf("평균값보다 작은 배열의 값: %d\n", arr[i]);
                maxOfSmallValue = arr[i];
            } else {
                // printf("평균값보다 큰 배열의 값: %d\n", arr[i]);
                minOfBigValue = arr[i];
                break;
            }
        }

        // printf("a: %d\n", a);
        // printf("b: %d\n", b);
        // 선택값과 근사치로 있는 양쪽의 값을 구했으면 
        // 근사치에서 가장 작은값을 빼고 가장 큰 값에서 큰사치를 뺴서 양쪽 거리의 격차값이 더 작은쪽이 센터
        if (abs((maxOfSmallValue - arr[0]) - (arr[count - 1] - maxOfSmallValue)) < abs((minOfBigValue - arr[0]) - (arr[count - 1] - minOfBigValue))) {
            isCenterHouse = maxOfSmallValue;
        } else {
            isCenterHouse = minOfBigValue;
        }
    }
    printf("Center House: %d\n", isCenterHouse);
}

int averageDistance(int arr[], int count) { // 평균 거리를 갖는 지역의 정수를 반환
    int sum = 0;
    int average = 0;
    for (int i = 0; i < count; i++) {
        sum += arr[i];
    }
    average = sum / count;
    // printf("Averrage Distance: %d\n", average);
    return average;
}

```



## ✔︎ 미션 3.
해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.

```c
// 아직 해결 중....
```


## ✔︎ 미션 4 (난이도 최상).
해당 코스에서 문제를 챌린저들을 위한것이기때문에 공개하면 안된다고 공지되어있어서 문제 내용은 제거합니다.
```c
#include <stdio.h>
#define HORIZONTAL 9
#define VERTICAL 8 // 세로길이는 별로 의미가 없네요?

/*
문제에서 원하는 출력값은 낙차값중에 가장 큰값입니다.
따라서 개별 박스의 낙차를 구하기보다는 해당 열에있는 박스중 가장 낙차값이 크게 나오는 숫자를
resultArr 배열에 저장하고
그리고 이 배열에서 가장 큰 값을 출력하는 방식으로 생각하고 코드를 작성했습니다.
90도 회전 어쩌구하는건 일단 무시했습니다.
*/

int dropTheBox[HORIZONTAL] = {7, 4, 2, 0, 0, 6, 0, 7, 0};
int resultArr[HORIZONTAL];

void vertical_drop(int dropTheBox[], int horizontal);

int main(void) {
    vertical_drop(dropTheBox, HORIZONTAL);
    int max = 0;
    printf("result array: ");
    for (int i = 0; i < HORIZONTAL; i++) { // 낙차값 배열중 가장 큰 값을 추출하는 코드 입니다.
        printf("%d ", resultArr[i]);
        if (max < resultArr[i]) {
            max = resultArr[i];
        }
    }
    printf("\n");
    printf("result: %d", max);
    return 0;
}

void vertical_drop(int dropTheBox[], int horizontal) {
    for (int i = 0; i < horizontal; i++) {
        int countMax = 0; // countMax 는 비교하려는 값(dropTheBox[i]) 보다 같거나 큰값이 몇개나 있는지 얻어오는 변수입니다.
        for (int j = i; j < horizontal; j++) { // 비교하려는 값보다 큰값들을 찾습니다.
            if (dropTheBox[i] <= dropTheBox[j] && dropTheBox[i] != 0) {
                countMax += 1;
            }
        }
        if (dropTheBox[i] == 0) { // arr[i] 가 0일경우 resultArr에 그대로 0을 넣어줍니다.
            resultArr[i] = 0;
        } else {
            // horizontal 은 90도 회전했을경우 떨어질 높이값이 됩니다.
            // 그래서 낙차값을 총 높이 - 아래박스가 현재 박스보다 크거나 같은 박스의 갯수 - 그리고 떨어질때마다 +1씩 낙차값이 올라가는데 자기 자신이 위치한 칸은 제외해야 하므로 i 를 빼줍니다.
            resultArr[i] = horizontal - countMax - i;
        }
    }
}
```
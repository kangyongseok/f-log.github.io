---
title: "알고리즘 - 기능개발"
categories: 
  -  Algorithm
tags: 
    - Algorithm
    - coding
    - javascript
    - 프로그래머스
    - 알고리즘풀이
    - 연습
    - 스택/큐
toc: true
toc_sticky: true
comments:  true
---

## 문제 설명
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.
  
또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.
 
먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.  

## 제한 사항
- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

## 입출력 예
| progresses               | speeds             | return    |
|--------------------------|--------------------|-----------|
| [93, 30, 55]             | [1, 30, 5]         | [2, 1]    |
| [95, 90, 99, 99, 80, 99] | [1, 1, 1, 1, 1, 1] | [1, 3, 2] |


## 말 풀이
작업의 현재 진행률과 해당 작업의 속도가 배열로 주어지면 며칠째에 몇개의 기능이 함꼐 배포되는지 리턴하는 문제이다.
만약 앞서 배포중인 기능이 있다면 후속배포기능이 아무리 빨리 완료가 되더라도 앞선 기능 배포전까지는 배포가 될 수 없고 앞선기능이 배포되면 동시에 배포되는 형식이다.
  
자바스크립트로 보자면 동기적 동작이고 프로그래밍적으로 보자면 FIFO(First In First Out) 인 스택문제로 볼 수 있다.

즉 진행률의 배열갯수와 순서는 작업속도의 배열의 갯수와 순서가 동일하다고 볼 수 있다. 그러면 해당 작업이 며칠째에 배포되는지에 대한 값들을 쉽게 구할 수 있고 그 값을 바탕으로 몇개의 기능이 한꺼번에 배포되는지 알 수 있다.

## 코드
```javascript
function solution(progresses, speeds) {
    var answer = [];
    let result = [];

    const updateDate = progresses.map((percent, i) => { // 각 프로세스별로 며칠째에 배포가되는지 구함
        let day = 0;
        for (let j = percent; j < 100; j = j + speeds[i]) {
            day = day + 1
        }
        return day
    })

    let prev = updateDate[0];
    let count = 0;
    updateDate.map((day, i) => { // 몇개의 기능들이 동시에 배포될 수 있는지 구함
        if (prev >= day) {
            count += 1;
        }
        if (prev < day) {
            prev = day
            result.push(count)
            count = 1;
        }
        if (i === updateDate.length - 1) {
            result.push(count)
        }
    })
    answer = result
    return answer;
}
```
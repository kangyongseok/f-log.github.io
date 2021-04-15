---
title: "카카오 인턴십 - 키패드 누르기"
categories: 
  -  Algorithm
tags: 
    - Algorithm
    - coding
    - javascript
    - 프로그래머스
    - 알고리즘풀이
    - 연습
    - 카카오 인턴십 2020
toc: true
toc_sticky: true
comments:  true
---

## 문제 설명
[문제 바로가기](https://programmers.co.kr/learn/courses/30/lessons/67256)

## 풀이
```javascript
function solution(numbers, hand) {
    var answer = '';
    const padMap = [[3, 1], [0, 0], [0, 1], [0, 2], [1, 0], [1, 1], [1, 2], [2, 0], [2, 1], [2, 2]];
    let leftMap = [3, 0]
    let rightMap = [3, 2]
    const result = numbers.map((num, i) => {
        if (num === 1 || num === 4 || num === 7) {
            leftMap = padMap[num]
            return 'L'
        }
        if (num === 3 || num === 6 || num === 9) {
            rightMap = padMap[num]
            return 'R'
        }
        if (num === 2 || num === 5 || num === 8 || num === 0) {
            const a = Math.abs(padMap[num][0] - leftMap[0]) + Math.abs(padMap[num][1] - leftMap[1])
            const b = Math.abs(padMap[num][0] - rightMap[0]) + Math.abs(padMap[num][1] - rightMap[1])
            if (a < b) {
                leftMap = padMap[num]
                return 'L'
            }
            if (a > b) {
                rightMap = padMap[num]
                return 'R'
            }
            if (a === b) {
                if (hand === 'right') {
                    rightMap = padMap[num]
                    return 'R'
                } else {
                     leftMap = padMap[num]
                    return 'L'
                }
            }
        }
        
    })
    answer = result.join('')
    
    return answer;
}
```

일단 의식의 흐름대로 풀었는데 너무웃긴다 이렇게 못생긴 코드라니   
- right: [1, 4, 7] = 'R'
- left: [3, 6, 9] = 'L'
- center: [2, 5, 8, 0] = 조건에 따라 다름

이문제는 사실 가운데숫자의 `'L', 'R'` 을 구분하는 문제가 아닐까 싶다. 어쩃든 양 사이드에있는 전화번호는 이미 왼손과 오른손의 영역으로 정해져있는 부분이다.
  
그리고 엄지손가락은 계속 이동하게되는데 다음에 이동해야할 키패드와 가장 가까운 손가락이 움직이는것이 첫번째 조건이고 만약 이동거리가 동일하다면 두번째인자로 넘어온 위치가 우선시하게된다.
  
각 키패드의 좌표를 미리 정해놓고 손가락이 위치한 좌표와 다음으로 이동할 좌표의 거리를 구해 왼손과 오른손의 위치를 비교하여 값을 리턴하고 해당 손가락의 좌표를 바꿔주는식으로 작성하였다.
  
## 개선한 풀이
의식의 흐름대로 풀이를 진행하다보니 아무래도 가독성도 안좋고 코드의 라인수가 꽤 길어졌는데 이런 부분들을 개선한 풀이이다.
```javascript

```
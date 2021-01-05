---
title: "알고리즘 - 두 개 뽑아서 더하기"
categories: 
  -  Algorithm
tags: 
    - Algorithm
    - coding
    - javascript
    - 프로그래머스
    - 알고리즘풀이
    - 연습
toc: true
toc_sticky: true
comments:  true
---

## 두 개 뽑아서 더하기
정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

> 제한사항
> numbers의 길이는 2 이상 100 이하입니다 
> numbers의 모든 수는 0 이상 100 이하입니다.

### 입출력 예
**numbers**  
[2, 1, 3, 4, 1]
  
**result**  
[2, 3, 4, 5, 6, 7]

### 입출력 예 설명
- 2 = 1 + 1 입니다. (1이 numbers에 두 개 있습니다.)
- 3 = 2 + 1 입니다.
- 4 = 1 + 3 입니다.
- 5 = 1 + 4 = 2 + 3 입니다.
- 6 = 2 + 4 입니다.
- 7 = 3 + 4 입니다.
- 따라서 [2,3,4,5,6,7] 을 return 해야 합니다.

### 글코딩
해당 문제는 N0 ~ Nn 개의 숫자 배열이 주어졌을때 서로 다른 인덱스끼리의 합의 숫자를 배열에 담아 오름차순으로 정렬 후 리턴하라는 문제이다.  
- (N0 + N1), (N0 + N2), (N0 + N3), ..., (N0 + Nn);  
- (N1 + N2), (N1 + N3), (N1 + N4), ..., (N1 + Nn);  
.  
.  
.  

- 좌항 Nn 은 배열을 순환하는데 이때 자기 자신 인덱스위치의 이후부터만 루프를 돈다.  
- 우항 Nn 은 좌항의 인덱스값을 제외한 나머지값들이 매 배열순환마다 1씩 증가하는 값을 갖는다.
- 이 값들의 각각의 합을 빈 배열에 넣는데 이때 중복되는 값이 있다면 넣지않는다
- 이 빈 배열에 값이 모두 들어오면 오름차순으로 정렬하여 최종 결과배열을 리턴한다.

### Code
```javascript
function solution(numbers) {
    var answer = [];

    for(let i = 0; i < numbers.length - 1; i++) { // 좌항의 값
        for(let j = i + 1; j < numbers.length; j++) { // 우항의 값
            var sum = numbers[i] + numbers[j];
            if(!answer.includes(sum)) answer.push(sum); // 결과 배열에 중복되는 값이 없으면 push
        }
    }

    return answer.sort((a, b) => a-b); // 오름차순으로 정렬
}
```

---
title: "알고리즘 - 다트게임"
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

## 문제 설명
  
카카오톡 게임별의 하반기 신규 서비스로 다트 게임을 출시하기로 했다. 다트 게임은 다트판에 다트를 세 차례 던져 그 점수의 합계로 실력을 겨루는 게임으로, 모두가 간단히 즐길 수 있다.
갓 입사한 무지는 코딩 실력을 인정받아 게임의 핵심 부분인 점수 계산 로직을 맡게 되었다. 다트 게임의 점수 계산 로직은 아래와 같다.  

다트 게임은 총 3번의 기회로 구성된다.  
각 기회마다 얻을 수 있는 점수는 0점에서 10점까지이다.  
점수와 함께 Single(S), Double(D), Triple(T) 영역이 존재하고 각 영역 당첨 시 점수에서 1제곱, 2제곱, 3제곱 (점수1 , 점수2 , 점수3 )으로 계산된다.  
옵션으로 스타상(*) , 아차상(#)이 존재하며 스타상(*) 당첨 시 해당 점수와 바로 전에 얻은 점수를 각 2배로 만든다. 아차상(#) 당첨 시 해당 점수는 마이너스된다.  
스타상(*)은 첫 번째 기회에서도 나올 수 있다. 이 경우 첫 번째 스타상(*)의 점수만 2배가 된다. (예제 4번 참고)  
스타상(*)의 효과는 다른 스타상(*)의 효과와 중첩될 수 있다. 이 경우 중첩된 스타상(*) 점수는 4배가 된다. (예제 4번 참고)  
스타상(*)의 효과는 아차상(#)의 효과와 중첩될 수 있다. 이 경우 중첩된 아차상(#)의 점수는 -2배가 된다. (예제 5번 참고)  
Single(S), Double(D), Triple(T)은 점수마다 하나씩 존재한다.  
스타상(*), 아차상(#)은 점수마다 둘 중 하나만 존재할 수 있으며, 존재하지 않을 수도 있다.  
0~10의 정수와 문자 S, D, T, *, #로 구성된 문자열이 입력될 시 총점수를 반환하는 함수를 작성하라.  

## 입력 형식
점수|보너스|[옵션]으로 이루어진 문자열 3세트.  
예) 1S2D*3T  

점수는 0에서 10 사이의 정수이다.  
보너스는 S, D, T 중 하나이다.  
옵선은 *이나 # 중 하나이며, 없을 수도 있다.  

## 출력 형식  
3번의 기회에서 얻은 점수 합계에 해당하는 정수값을 출력한다.  
예) 37

## 입출력 예제
예제	dartResult	answer	설명  
1	1S2D*3T	37	11 * 2 + 22 * 2 + 33  
2	1D2S#10S	9	12 + 21 * (-1) + 101  
3	1D2S0T	3	12 + 21 + 03  
4	1S*2T*3S	23	11 * 2 * 2 + 23 * 2 + 31  
5	1D#2S*3S	5	12 * (-1) * 2 + 21 * 2 + 31  
6	1T2D3D#	-4	13 + 22 + 32 * (-1)  
7	1D2S3T*	59	12 + 21 * 2 + 33 * 2  

## 문제 풀이 설명
일단 이건 좀 하드하게 풀었다.  
어떤 규칙성이나 어떤 기준으로 코드를 작성해야할지 잘 보이지 않아서 조금 애를 먹었다.  
  
일단 주어진값은 문자열 형태로 주어지고 이걸 배열화 시켜야만한다. 카카오 해설을 보니 정규표현식을 사용해서 푼사람들도 있다는데 이 하드하게푼걸 다시 정규표현식을 사용해서 풀어볼 생각이다. 어쨋든 배열화 시킨 문자열을 보너스기준으로 코드가 실행되도록하였다.
  
S 일때 T 일때 D 일때  
  
우선 이 다트게임의 문자열은 가장 긴 길이가 12자가 될수있다.
```javascript
10S*10D#10T*
```

가장 짧은 길이는 6자가 된다
```javascript
1S2D3T
```

한자릿수 점수만 있었다면 좀 쉬웠겠지만 10이라는 두자릿수가 있다보니까 코드가 좀더 복잡해지고 길어졌던것같다. 실제로 문제를 풀었던 사람들중에서도 전체 실행코드는 통과했는데 테스트는 통과 못하는경우가 바로 이 10에대한 처리가 제대로 되지 않아서다.
  
일단 나는 `[0, 0, 0]` 으로 이루어진 빈 배열을 만들었고 조건에따라 각각의 계산된 점수를 저 배열에 0대신 치환되도록 하였다.  

그리고 배열의 값을 모두 더해서 값을 리턴하게 만들었다.

## 문제 풀이 코드
```javascript
function solution(dartResult) {
    var answer = 0;
    const arr = dartResult.split("")
    let result = [0, 0, 0];
    arr.map((t, i) => {
        switch(t) {
            case 'S': 
                let a = null;
                // 영역을 기준으로 -2위치에 1이 있다면 그 값을 10이기때문에 10을 하드코드로 넣어주었다.
                if (Number(arr[i - 2]) === 1) a = Math.pow(10, 1)
                // 그 이외의 경우에는 영역보다 이전값이 무조건 점수 이기때문에 거듭제곱을 진행했다.
                if (Number(arr[i - 2]) !== 1) a = Math.pow(arr[i - 1], 1)
                // 영역문자열의 위치에 따라 몇번째 합의 값인지 넣어주도록 하였다.
                if (i <= 2) result.splice(0, 1, a)
                if (i > 2 && i < 5 && dartResult.length <= 7) result.splice(1, 1, a)
                if (i > 2 && i < 6 && dartResult.length > 7) result.splice(1, 1, a)
                if (i >= 5 && dartResult.length <= 7) result.splice(2, 1, a)
                if (i >= 6 && dartResult.length > 7) result.splice(2, 1, a)
                break;
            case 'T':
                let b = null;
                if (Number(arr[i - 2]) === 1) b = Math.pow(10, 3)
                if (Number(arr[i - 2]) !== 1) b = Math.pow(arr[i - 1], 3)
                if (i <= 2) result.splice(0, 1, b)
                if (i > 2 && i < 5 && dartResult.length <= 7) result.splice(1, 1, b)
                if (i > 2 && i < 6 && dartResult.length > 7) result.splice(1, 1, b)
                if (i >= 5 && dartResult.length <= 7) result.splice(2, 1, b)
                if (i >= 6 && dartResult.length > 7) result.splice(2, 1, b)
                break;
            case 'D':
                let c = null;
                if (Number(arr[i - 2]) === 1) c = Math.pow(10, 2)
                if (Number(arr[i - 2]) !== 1) c = Math.pow(arr[i - 1], 2)
                if (i <= 2) result.splice(0, 1, c)
                if (i > 2 && i < 5 && dartResult.length <= 7) result.splice(1, 1, c)
                if (i > 2 && i < 6 && dartResult.length > 7) result.splice(1, 1, c)
                if (i >= 5 && dartResult.length <= 7) result.splice(2, 1, c)
                if (i >= 6 && dartResult.length > 7) result.splice(2, 1, c)
                break;
            case '*':
                // 몇번째 던진 다트의 값인지 필요한 이유는 바로 이 옵션때문이다.
                // 해당 옵션의 위치에 따라 몇번째값까지 2배를 해줄건지 계산이 필요하다
                if (i < 3) result.splice(0, 1, result[0] * 2)
                if (i > 3 && i <= 5) {
                    result.splice(0, 1, result[0] * 2)
                    result.splice(1, 1, result[1] * 2)
                }
                if (i > 5) {
                    result.splice(1, 1, result[1] * 2)
                    result.splice(2, 1, result[2] * 2)
                }
                break;
            case '#':
                if (i < 3) result.splice(0, 1, result[0] * -1)
                if (i > 3 && i <= 5) {
                    result.splice(1, 1, result[1] * -1)
                }
                if (i > 5) {
                    result.splice(2, 1, result[2] * -1)
                }
                break;
            default:
                break;
        }
    })
    answer = result.reduce((a, b) => a + b)
    return answer;
}
```
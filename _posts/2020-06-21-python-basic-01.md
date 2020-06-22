---
title: "Python Basic 01"
categories: 
  -  python
tags: 
    - develop
    - python
    - study
    - backend
toc: true
toc_sticky: true
comments:  true
---

## 실습문제01
socre 리스트에 다섯개의 성적(정수)를 입력받아 다음과 같이 출력  
- 성적을 입력받을때는 반드시 for 구문을 사용
- 파이썬 내장 함수를 적절히 이용
```
성적을 입력하시오: 90
성적을 입력하시오: 88
성적을 입력하시오: 75
성적을 입력하시오: 93
성적을 입력하시오: 80

입력한 성적들 : [90, 88, 75, 93, 80]
최고성적 : 93
최저성적 : 74
평균 : 85.20
```

## 풀이
```python
score = [] # 성적을 담을 빈 배열 생성
for i in range(5):
    data = int(input('성적을 입력하시오 : '))
    score.append(data)

print('입력한 성적들 : ', score)

print('최고성적 : ', max(score))
print('최저성적 : ', min(score))

average = sum(score) / len(score)

print('평균 : %.2f' % average)
```

## 실습문제02
A 고등학교 어떤 반 학생들이 5명의 번호와 국어, 영어, 수학 성적이 다음과 같다.  
5명 각자의 평균을 출력하시오 
('사전' 과 '리스트' 이용)    
| 번호 | 국어 | 영어 | 수학 |
|---|:---:|:---:|---:|
| 1 | 80 | 90 | 86 |
| 2 | 78 | 88 | 85 |
| 3 | 85 | 85 | 92 |
| 4 | 70 | 69 | 65 |
| 5 | 90 | 95 | 100 |


## 실습
```python
score = {
    1: [80, 90, 86],
    2: [78, 88, 85],
    3: [85, 85, 92],
    4: [70, 69, 65],
    5: [90, 95, 100],
}

for key, value in score.items():
    print(key, '번 : ', sum(value) / len(value))
```
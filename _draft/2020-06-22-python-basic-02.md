---
title: "Python Basic 02"
categories: 
  -  python
tags: 
    - develop
    - python
    - study
    - backend
    - function
    - lambda
    - class
    - module
toc: true
toc_sticky: true
comments:  true
---

## python 함수와 모듈

### lambda 함수
```python
x = [1, 3, 5, 7, 9]
result = filter(lambda x: x>5, x)
print(result)
print(list(result))
```

### 실습문제
초과근무시간과 근무시간에 따른 급여를 함수로 만들기

```python 
def week_salary(hour, pay):
    total_pay = hour * pay
    if hour > 12:
        additional_pay = (hour - 12) * pay * 0.3
        total_pay += additional_pay
    return total_pay

working_hour = int(input('근무 시간을 입력하시오 : '))
pay_per_hour = int(input('시간당 수당을 입력하시오 : '))

answer = week_salary(working_hour, pay_per_hour)
print()
print(answer)
```

## python class
클래스를 활용하여 강아지 정보 객체로 표현하기

```python
class Dog:
  def __init__(self, name, age):
      self.name = name
      self.age = age

  def bark(self):
      print(self.name, 'is bark')


x = Dog('Jack', 3)
y = Dog('cholong', 2)

x.bark()
y.bark()

print(x.name, 'is', x.age, 'years old')
print(y.name, 'is', y.age, 'years old')
```
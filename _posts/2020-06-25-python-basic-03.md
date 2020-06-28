---
title: "Python Basic 03"
categories: 
  -  python
tags: 
    - develop
    - python
    - study
    - class
    - Inhetirance
toc: true
toc_sticky: true
comments:  true
---

## Python 상속
상속은 하나의 클래스가 다른 클래스에 정의된 변수나 메소드를 모두 받아서 사용가능한것이라고 볼 수 있다.  

하위클래스, 자식클래스, 상속자, 부모자식관계 등 여러가지 용어로 불릴 수 있다.  

### 상속 예제
```python
class PartyAnimal:
    x = 0
    name = ""
    def __init__(self, name):
        self.name = name
        print(self.name, "constructed")
    
    def party(self):
        self.x = self.x + 1
        print(self.name, "party count", self.x)

class FootballFan(PartyAnimal):
    points = 0
    def touchdown(self):
        self.points = self.points + 7
        self.party()
        print(self.name, "points", self.points)


s = PartyAnimal("Sally") # Sally constucted
s.party() # Sally party count 1

j = FootballFan("Jim") # Jim constucted
j.party() # Jim party count 1
j.touchdown() # Jim party count 2 / Jim points 7
```
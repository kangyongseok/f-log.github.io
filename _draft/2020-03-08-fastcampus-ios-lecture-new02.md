---
title: "FastCampus New IOS Lecture - 02"
categories: 
  - study
tags: 
    - lecture
    - online
    - fastcampus
    - IOS
    - Swift
    - study
toc: true
toc_sticky: true
comments:  true
---

## Optional
값이 있으면 값을 표현 없으면 `nil` 표현
  
``` swift

var carName: String?
carName = "아우디"

// 고급기능 4가지

// Forced unwrapping - 억지로 까보기
// Optional binding (if let) - 부드럽게 까보자 1
// Optional binding (guard) - 부드럽게 까보자 2
// Nil coalescing - 까봤더니 없으면 디폴트 값을 줘보자

print(carName!) // carName 에 값이 없으면 경고를 반환

if let unwrappingCarName = carName {
    print(unwrappingCarName)
} else {
    print("Car Name 없다")
}

func printParsedInt(from: String) {
    if let parsedInt = Int(from) {
        print(parsedInt)
    } else {
        print("Int로 컨버팅 안된다")
    }
}
printParsedInt(from: "50")
printParsedInt(from: "안녕")


// guard 조건이 맞으면 그대로 리턴을 생략할 수 있다. 조건이 틀리때의 구문만 작성해주면됨
func printParsedIntGuard(from: String) {
    guard let parsedInt = Int(from) else {
        print("Int 로 안된다")
        return
    }
    print(parsedInt)
}
printParsedIntGuard(from: "30")


// carName이 값이 없으면 "모델3" 값을 기본값으로 가짐
let myCarNAme: String = carName ?? "모델3"
```
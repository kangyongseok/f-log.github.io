---
title: "FastCampus IOS (01)"
categories: 
  - study
tags: 
    - lecture
    - online
    - fastcampus
    - IOS
    - Swift
    - networking
    - study
toc: true
toc_sticky: true
comments:  true
---

음 일단 정리하기전에 반성을한다. 이 강의는 약 2년전에 결제했던 강의로 온라인강의의 장점인 평생 수강과 업데이트를 지속적으로 해준다는 장점을 보고 수강신청을 했었다. 그러나 계속 띄엄띄엄 듣는바람에 제대로 완강을 한적이 없었고 지금에와서 정신차리고 강의를 제대로 들으면서 정리를 해 나가려고 한다. 
  
UI 부터 듣는게 당연하다 생각해서 순서대로 들었었는데 이번엔 네트워킹 강의부터 시작하려고 한다. 오늘이 그 첫번째 정리의 날이다. 이 정리와 공부가 꾸준히 이어져서 우선은 가장 유명한 ToDo List앱을 구현하고 그 다음엔 머릿속에 계획중인 프로젝트를 하나 진행해서 앱을 출시할 계획이다. 
  
강좌는 최소 하루에 3개 이상은 들을 생각이고 (한 강좌당 시간은 10분~20분 사이정도 된다.) 키워드만 정리하려고 한다.

## 2020-02-09 내용정리

#### IOS 네트워크 연결

**용어정리**  

URLSession  
GCD라이브러리  
- userInteractive (가장 높은 우선순위)
- userInitiated
- default
- utility
- background (가장 시간이 오래 걸리는 작업)
``` java
// Queue
DispatchQueue.main.async {}
DispatchQueue.global(qos: .background).async {}
DispatchQueue.global(qos: .userInteractive).async {}

// Global Queue
DispatchQueue.global(qos: .userInteractive).async {}
DispatchQueue.global(qos: .userInitiated).async {}
DispatchQueue.global(qos: .utility).async {}
DispatchQueue.global(qos: .background).async{}

// Custom Queue
let concurrentQueue = DispatchQueue(label: "concurrent", qos: .background, attributes: .concurrent)
let serialQueue = DispatchQueue(label: "serial", qos: .background)
```
  
동시성(Concurrency)  
URLSessionConfiguration  
URLSessionTask  
Queue, Sync & Async  
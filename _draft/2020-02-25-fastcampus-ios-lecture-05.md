---
title: "FastCampus IOS (05) - Firebase"
categories: 
  - study
tags: 
    - lecture
    - online
    - fastcampus
    - IOS
    - Swift
    - firebase
    - study
toc: true
toc_sticky: true
comments:  true
---

## 데이터베이스에 저장한 값 불러오기

**Int**
``` swift
rootRef.child("int").observeSingleEvent(of: .value) {
    snapshot in print("---> \(snapshot)")
    
    let value = snapshot.value as? Int ?? 0
    DispatchQueue.main.async {
        self.firstDataLabel.text = "\(value)"
    }
}
```

**Double**
``` swift
rootRef.child("double").observeSingleEvent(of: .value) {
    snapshot in print("---> \(snapshot)")
    
    let value = snapshot.value as? Double ?? 0.0
    DispatchQueue.main.async {
        self.firstDataLabel.text = "\(value)"
    }
}
```

**observeSingleEvent**  
데이터 베이스의 정보가 변해도 view 에서 보여주는 데이터는 변하지 않는다.  
한번만 호출한다.


**observe**  
데이터 베이스의 정보가 변경될때마다 실시간으로 변한다. 즉 콜백을 계속 호출한다.



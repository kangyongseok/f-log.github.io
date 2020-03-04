---
title: "FastCampus IOS (03) - Firebase"
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


## Firebase
앱과 관련하여 서버쪽 관련 작업을 쉽게 구성하 나갈 수 있도록 여러가지 기능과 툴들을 제공해주는 사이트

## Cocoapod
IOS 관련 패키지 관리 라이브러리 npm 과 비슷한것

## Firebase 구성하기
프로젝트를 하나 생성하고 영상과 Firebase 공식 사이트를 보고 진행사항대로 따라했는데... 에러가 발생한다.
`import Firebase` 가 에러가 발생한다.`(No such module 'Firebase' error)` 일단 오늘 새벽공부는 여기까지... 영상 시청하는데 세팅부터 막히네..

------------

음 어찌 어찌 해결한것같다.   
`import Firebase` 에 대한 모듈을 못찾는다는 이슈는 여러가지 이유가 있어서 하나하나 다 적용해보면서 뭐가 문제인지 찾는 수밖에 없다고 다른 개발자에게 들었던것같은데 나같은경우는 이 앱에 대한 `signing?` 이었나... 내 애플 계정을 등록을 해줘야했는데 그게 없어서 못찾는것같았다.  

이게 어떤 관계가 있어서 `Firebase` 를 `import` 하는데 모듈을 못찾는다는 에러가 발생했는지는 정확한 상관관계는 잘 모르겠다. 어쨋든 `Firebase` 를 `import` 하려면 개발자 계정을 등록하고 device 등록을 진행하니 빌드도 정상적으로되고 에러메세지가 뜨던 부분도 사라졌다.

-------------- 

강의자료와  Firebase 에 안내된 IOS 앱과 연동 방법을 프로젝트 생성후 절차대로 진행하고 
연결 코드를 작성하여 연결까지 완료 하였다.

**Pods/podfile**
``` swift
<!-- 아래 코드 추가 / 추가될때마다 pod install 을 실행해 줘야함 -->
pod 'Firebase/Core'
pod 'Firebase/Analytics'
pod 'Firebase/Database'
```

```$ pod install```
  
    
      

**AppDelegate.swift**
``` swift
....

import Firebase

...

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.
    FirebaseApp.configure() // <-- 해당코드 추가
    return true
}

....
```

**ViewController.swift**
``` swift

import UIKit
import Firebase
import FirebaseDatabase // <-- 강의와 다르게 이부분을 추가해줘야 에러가 발생하지 않음

class ViewController: UIViewController {

    @IBOutlet weak var firstDataLabel: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        let rootRef = Database.database().reference()
        rootRef.child("test").observeSingleEvent(of: .value) {
            snapshot in print("---> \(snapshot)")
            
            let firstData = snapshot.value as? String ?? "nothing"
            DispatchQueue.main.async {
                self.firstDataLabel.text = firstData
            }
        }
    }
}
```


## 참고사이트
[Firebase realDataBase IOS starter](https://firebase.google.com/docs/database/ios/start?hl=ko)  

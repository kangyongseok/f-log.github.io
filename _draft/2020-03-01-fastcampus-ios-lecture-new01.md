---
title: "FastCampus New IOS Lecture - 01"
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

`FastCampus` 온라인 강좌를 수강중이다. 기존 `모바일올인원패키지` 강좌에 IOS 파트를 듣고있었는데 2, 3년전 신청했던 강좌다보니까 내용이 예전 버전에 머물러있어서 예제를 따라하는도중에 자잘한 버그들이 많이 발생했다.  

그대로 진행하기에는 학습보단 버그잡는데 시간이 더 오래 걸릴것같아 패캠쪽에 메일로 문의를 넣었더니 대박사건!  
현재 2020년 기준으로 IOS 만 따로 강의를 제작해서 판매중인데 이 수강권을 무료로 제공해준다는것이다.  

그래서 당연히 오케이를 외치고 무료수강권으로 등록하여 다시 수강하며 블로그에 내용을 정리하려고한다.  

4월달은 되어야 모든 강좌가 공개되지만 우썬 빠르게 지금까지 나온 강좌 학습하고 강사말대로 우선 간단한거라도 앱을 만들어보려고한다. 당연히 까다로운 앱 심사떄문에 출시는 못하겠지만 우선 이것저것 만져보며 xcode 와 swift 언어에 대한 경험을 쌓아 나가야 할것같다.  

최근 회사에는 신입이긴하지만 IOS 개발자도 입사했기때문에 충분히 막히는 부분은 물어보면서 해결해 나갈 수 있을것같다.

-------

## View Controller
페이지당 하나씩 필요한 화면을 컨트롤하는 부분

## 간단한 alert 띄우기 예제
``` swift
@IBAction func hello(_ sender: Any) {
    let alert = UIAlertController(title: "Hello", message: "My Frist App!", preferredStyle: .alert)
    let action = UIAlertAction(title: "ok", style: .default, handler: nil)
    alert.addAction(action)
    present(alert, animated: true, completion: nil)
}
```

뭐 일단 아무것도 모르는 상태에서 저 코드를 내멋대로 일단 해석을 해본다.  

자바스크립트에서는 alert 은 저렇게 변수로 선언할 필요없이 `alert('message')` 로 띄울 수 있는데 swift 에서는 `UI~` 로 시작하는 함수를 사용해야하는것같다.  

>UIAlertController (title, "text", message: "message", preferredStyle: .alert or .actionSheet)  

`UIAlertController` 에 파라미터로 넘겨줄 수 있는 요소에는 `title, message, preferredStyle` 일단 이 세가지가 있는것같고   
`title`은 `alert` 의 상단 제목을 보여줄거고   
`message` 는 `alert`의 메세지가 될것같다.   
`preferredStyle` 은 잘 모르겠다. 아마 `alert`의 종류를 정해줄 수 있을것같다.   
- (.alert, .actionSheet) 두가지 를 설정 할 수 있다.

>UIAlertAction(title: " ", style: .default, handler: nil)  

`UIAlertAction` 에 파라미터로 넘기는 요소는 `title, style, handler` 가 있고 action 이 들어간걸보니 아마 alert에 있는 버튼에 대한 작동을 제어하는것같다. 
  
`title` 은 alert 버튼의 이름일거고   
`style` 은 alert 의 동작 그리고   
`handler` 는 버튼을 클릭한 이후의 동작을 제어하는 파라미터인것같다.  

>alert.addAction(action)
  
alert 에 액션을 추가한다는 메서드를 사용하고 파라미터로 정의한 액션을 넘겨주는것같다.  
  

>present(alert, animated: true, completion: nil)
  
alert 을 화면에 띄우는 함수인것같다.

## 앱 동작 방식의 이해
- 앱은 오브젝트로 구성
- 오브젝트 끼리 서로 메세지 보냄
- 앱은 이벤트에 의해 프로세스 동작함(그전까지는 자고있는 상태)
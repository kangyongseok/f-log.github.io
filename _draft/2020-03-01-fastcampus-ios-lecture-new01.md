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

4월달은 되어야 모든 강좌가 공개되지만 우썬 빠르게 지금까지 나온 강좌 학습하고 강사말대로 우선 간단한거라도 앱을 만들어보려고한다. 당연히 까다로운 앱 심사때문에 출시는 못하겠지만 우선 이것저것 만져보며 xcode 와 swift 언어에 대한 경험을 쌓아 나가야 할것같다.  

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

## 로컬변수 / 인스턴스변수

``` swift
import UIKit

class ViewController: UIViewController {
    
    var currentValue = 0 // 인스턴스변수 - 오브젝트 내부에서 사용

    @IBOutlet weak var priceLable: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        refresh()
    }

    @IBAction func showAlert(_ sender: Any) {
        let message = "가격은 \(currentValue) 입니다." // 로컬변수 - 메소드 내부에서 사용
        let alert = UIAlertController(title: "Hello", message: message, preferredStyle: .actionSheet)
        let action = UIAlertAction(title: "ok", style: .default, handler: nil)
        alert.addAction(action)
        present(alert, animated: true, completion: nil)
        
        refresh()
    }
    
    func refresh() {
        let randomPrice = arc4random_uniform(10000) + 1 // 로컬변수
        currentValue = Int(randomPrice)
        priceLable.text = "₩\(currentValue)"
    }
}
```

## Swift Flow Control

**while**
``` swift
// -- While
// 조건 맞으면 코드 진행
var i = 10
while i < 10 {
    print(i)
    
//    if i == 5 {
//        break
//    }
    i += 1
}


print("---- repeat")
// 코드진행 조건확인
i = 10
repeat {
    print(i)
    i += 1
} while i < 10
```

`repeat - while` 은 `javascript` 의 `do - while` 문과 같은동작인것같다.


**for**
``` swift
import UIKit
import Foundation

let closedRange = 0...10 // 0~10 까지 표현
let halfClosedRange = 0..<10 // 0~9 까지 표현

var sum = 0
for i in halfClosedRange {
    print("---> \(i)")
    sum += i
}

print("---> total sum: \(sum)")


var sinValue: CGFloat = 0
for i in closedRange {
    sinValue = sin(CGFloat.pi/4 * CGFloat(i))
}

for i in closedRange {
    if i % 2 == 0 {
        print("---> 짝수: \(i)")
    }
}

for i in closedRange where i % 2 == 0 {
    print("---> 짝수: \(i)")
}


for i in closedRange {
    if i == 3 {
        continue
    }
    
    print("---> \(i)")
}
```

**switch**
``` swift
let num = 10

switch num {
case 0:
    print("---> 0입니다.")
case 0...10:
    print("---> 0에서 10사이 입니다.")
case 10:
    print("---> 10입니다.")
default:
    print("---> 나머지 입니다")
}

let coordinate = (x: 10, y: 10)

switch coordinate {
case (0, 0):
    print("---> 원점")
case (let x, 0):
    print("---> x축, x:\(x)")
case (let x, let y) where x == y:
    print("---> x 랑 y랑 같음 x, y = \(x), \(y)")
default:
    print("---> 좌표 어딘가")
}
```

## Function
**func**

``` swift

// 함수호출기본형
func printName() {
    print("---> May name is jaon")
}
printName()



// 함수호출 파라미터형
func printMultipleOfTen(value: Int) {
    print("\(value) * 10 = \(value * 10)")
}
printMultipleOfTen(value: 5)


// 함수호출 다중파라미터형
func printMultiplePriceCount(price: Int, count: Int) {
    print("\(price * count)")
}
printMultiplePriceCount(price: 1000, count: 3)


// _ : 파라미터 이름없이 받겠다는것
func printTotalPrice(_ price: Int, _ count: Int) {
    print("Total Price: \(price * count)")
}
printTotalPrice(1500, 5)


// 함수호출 default 파라미터
func printTotalPriceWithDefaultValue(price: Int = 1500, count: Int) {
    print("---> Total Price: \(price * count), count: \(count)")
}
printTotalPriceWithDefaultValue(count: 3)
printTotalPriceWithDefaultValue(count: 15)
printTotalPriceWithDefaultValue(count: 1)
printTotalPriceWithDefaultValue(count: 0)

printTotalPriceWithDefaultValue(price: 2000, count: 1)


// -> Int : 반환할 값의 타입 return 이 있어야 한다.
func totalPrice(price: Int, count: Int) -> Int {
    let totalPrice = price * count
    return totalPrice
}

let price = totalPrice(price: 1000, count: 5)
price

```

**function 고급기능**
``` swift
func add(_ a: Int, _ b: Int) -> Int {
    return a + b
}

func subtract(_ a: Int, _ b: Int) -> Int {
    return a - b
}


var function = add
function(4, 2)
function = subtract
function(4, 2)

// 함수 파라미터로 함수를 넘기는것이 가능
func printResult(_ function: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    let result = function(a, b)
    print(result)
}
printResult(add, 10, 5)
printResult(subtract, 10, 5)
```
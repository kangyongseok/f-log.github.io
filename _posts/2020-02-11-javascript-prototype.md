---
title: "javascript prototype"
categories: 
  - javascript
tags: 
    - javascript
    - prototype
    - study
toc: true
toc_sticky: true
comments:  true
---

오늘 팀내에 서버 개발자에게 질문을 하나 받았다. 주말사이에 자바스크립트를 강의를 보고 공부를 했다고 하는데 이해가 안된다는 부분이 있었다고한다. 
  
> 질문 : 12.toString() 은 에러를 배출하는데   
> const num12 = 12  
> num12.toString() 은 동작하는 이유를 모르겠다.  
> 그럼 12 와 num12 는 서로 다른거냐  

여기에 대한 답변을 간단하게 해 주었는데 12에 바로 toString을 쓰는건 12는 아직 자바스크립트의 메모리에 저장된 객체가 아니기 때문에 메서드를 사용할 수 없고 num12 는 변수로 저장되어 자바스크립트의 메모리에 할당되어있는 값이기 때문에 toString() 을 사용 할 수 있다.   

라고 설명을 해 주었는데 음 생각해보니 정확한 답변은 아니었단 생각이 들었고 질문을 받은김에 한번 정리를 좀 해보려고 한다.  

## 자바스크립트는 프로토타입 기반 객체지향 프로그래밍 언어이다.

``` javascript
 const person = {
     age: 30,
     name: '홍길동'
 }
//  hasOwnProperty 가 person 에는 정의되지않았지만 사용 가능하다.
 console.log(person.hasOwnProperty('age')) // true

 console.dir(person)
/*
__proto__:
constructor: ƒ Object()
hasOwnProperty: ƒ hasOwnProperty() 여기에 있다.
isPrototypeOf: ƒ isPrototypeOf()
propertyIsEnumerable: ƒ propertyIsEnumerable()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString()
valueOf: ƒ valueOf()
__defineGetter__: ƒ __defineGetter__()
__defineSetter__: ƒ __defineSetter__()
__lookupGetter__: ƒ __lookupGetter__()
__lookupSetter__: ƒ __lookupSetter__()
get __proto__: ƒ __proto__()
set __proto__: ƒ __proto__()
*/

```

팀내에 백엔드 개발자는 다른거 배운거없이 python 언어를 배우고 바로 이 회사에 백엔드신입 개발자로 들어왔기때문에 자바스크립트나 프론트쪽에 대해서는 자세히 알지 못했다.  
  
그래서 아마 자바스크립트의 동작방식에 대해서 이해하기 좀 어렵고 힘들었을 수 있겠다. 보통 다른 언어에서는 객체 생성 이전에 클래스를 정의하고 이를 통해 객체를 생성하는 방식을 사용한다. 그러나 자바스크립트에서는 클래스없이도 객체생성이 가능하다.
  
자바스크립트의 모든 객체는 자신의 부모역할을 담당하는 객체와 연결이 되어있고 이것은 객체지향의 상속 개념과 같이 부모 객체의 프로퍼티나 메소드를 상속받아 사용 가능하다.  
  
모든 객체는 위의 예제와 같이 프로토 타입을 가지며 객체는 ```Object.prototype``` 함수는 ```Function.prototype``` 을 가르킨다.

## 결론

다시한번 서버 개발자가 했던 질문에 답변을 하자면  

> 12 는 자바스크립트에서 아직 아무런 객체도 아니기 때문에 prototype이 존재하지않아 .toString() 메소드를 사용할 경우 에러가 발생한다.  
> const num12 = 12 로 변수에 넣을경우 num12 는 객체가 되고 prototype이 생성되기때문에 .toString() 메소드 사용이 가능하다.

역시 내가 처음에 했던 설명은 완전 딴소리였다. 정확한 이유는 따로 있었고 그것은 객체 생성시 가지게되는 prototype 때문에 가능한 일이었다.  

## 참고
[PoiemaWeb - Prototype](https://poiemaweb.com/js-prototype)
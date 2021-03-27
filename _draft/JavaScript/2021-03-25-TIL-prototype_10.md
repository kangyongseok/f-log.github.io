---
title: "JavaScript 프로토타입"
categories: 
  - javascript
tags: 
    - 자바스크립트
    - 자바스크립트기본
    - 기본으로돌아가서
    - 생성자함수
    - 프로토타입
toc: true
toc_sticky: true
comments:  true
---

자바스크립트는 멀티패러다임 프로그래밍 언어로 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍 등을 지원하는 복합적인 내용을 갖고있는 언어라고 볼 수 있다.


## 프로토타입 

자바스크립트에서 객체지향 프로그래밍을 말할때 항상 조건부로 `프로토타입기반` 이라는 단어가 붙는다. 자바스크립트에서는 원시값을 제외한 나머지 모든값이 객체이고 객체이기때문에 원시값을 제외한 모든 값들은 프로토타입이라는 프로퍼티를 갖게된다.

자바스크립트에서는 이 프로토타입으로 상속을 구현할 수 있다.
```javascript
function Person(age) {
	this.age = age;
  	this.afterAge = function (after) {
    	return this.age + after
    }
}

const person1 = new Person(18)
const person2 = new Person(20)

console.log(person1.afterAge(2)) // 20
console.log(person2.afterAge(2)) // 22

console.log(person1.afterAge === person2.afterAge) // false
```

만약 위와같이 생성자함수 `Person` 을 생성하고 객체를 두번 호출하게되면 생성된 인스턴스는 각각 `afterAge` 라는 메소드를 갖고있게된다. 메소드의 역할은 동일하지만 별도의 메모리 공간을 차지하게되는 비효율적인 상황이 발생한다. 

이때 동일한 메소드를 생성된 인스턴스간에 공유가되면 메모리낭비도 발생시키지않고 효율적인 프로그래밍이 가능하다.

```javascript
function Person(age) {
	this.age = age;
}

Person.prototype.afterAge = function (after) {
  return this.age + after
}

const person1 = new Person(18)
const person2 = new Person(20)

console.log(person1.afterAge(2)) // 20
console.log(person2.afterAge(2)) // 22

console.log(person1.afterAge === person2.afterAge) // true
```

위 코드를 해석하자면 `person1` 과 `person2` 는 생성자함수 `Person` 에 `prototype` 을 상속받았기때문에 동일한 기능을 가진 메소드를 공유하여 사용할 수 있는데 이것때문에 프로토타입기반 객체지향 프로그래밍 이라고 말 할 수 있는것이다.

모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가지며 이 슬롯에 저장되는 프로토타입은 객체 생성방식에 의해서 결정된다. 즉 객체가 생성될때 객체의 생성방식에 따라 프로토타입이 결정되고 `[[Prototype]]` 이라는 내부 슬롯에 저장된다. 이 내부 슬롯에 직접 접근은 불가능하지만 `__proto__` 접근자 프로퍼티를 통해서 간접적으로는 접근이 가능하다.

그리고 프로토타입은 자신이 갖고있는 `constuctor` 프로퍼티를 통해 생성자 함수에 접근할 수 있고 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근 할 수 있다.

```javascript
const obj = {};
const parent = { a : 1 };

obj.__proto__; // getter 함수인 get __proto__ 가 호출
obj.__proto__ = parent; // setter 함수인 set __proto__ 가 호출

console.log(obj.a); // 1
```
위와같이 객체의 생성 할당이 이루어 졌을경우 접근자 프로퍼티인 `__proto__` 의 함수 getter 와 setter 가 동작해서 프로토타입을 취득하거나 교체한다.

```javascript
console.dir(obj)
// __proto__:
// 		a : 1
//		__proto__:
console.dir(parent)
// a: 1
// __proto__:
```

`__proto__` 접근자 프로퍼티는 객체가 직접 소유하는것이 아닌 `Object.prototype` 의 프로퍼티이다. 모든 객체는 상속을 통해서 `Object.prototype.__proto__` 접근자 프로퍼티를 사용할 수 있다.

```javascript
const person = { name: 'kang' };

// 객체가 직접 __proto__ 를 갖고있지 않다
console.log(person.hasOwnProperty('__proto__')); // false

console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__')); // {enumerable: false, configurable: true, get: ƒ, set: ƒ}
```

## 이렇게 사용하는 이유
프로토타입에 접근할때 이렇게 `__proto__` 접근자 프로퍼티로 간접적으로 불편하게 적용하는 이유가 뭘까? 그 이유는 상호참조에 의해서 무한 프로토타입 체인이 생성되는것을 방지하기 위해서다.

```javascript
const parent = {};
const child = {};
child.__proto__ = parent;
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

`__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는것은 권장되어지지 않으며  참조를 취득하고 싶을때는 getPrototypeOf, 프로토타입을 교체하고싶을때는 setPrototypeOf 를 사용하기를 권장한다.

## 함수객체에서의 prototype 프로퍼티
prototype 이라는 프로퍼티는 함수객체만이 소유하고있는 프로퍼티이다. 이 프로퍼티는 생성자함수가 생성할 인스턴스의 프로토타입을 가리킨다. 그리고 함수가 생성될때마다 모든 프로토타입은 자동으로 `constructor` 프로퍼티를 갖는데 이 프로퍼티는 소속된 함수를 가르킨다.

```javascript
function Person(name) {
	this.name = name;
}
const person1 = new Person('kang');

console.log(Person.prototype.constructor === Person); // true
console.log(person1.constructor === Person); // true
```
사실 위의 예제에서 실제로 `constructor` 를 갖고있는건 `Person.prototype` 이고 `person1` 의 객체는 [[Prototype]] 슬롯이 `Person.prototype` 과 연결되어있기때문에 `constructor` 를 사용하여 읽어오기가 가능하다.

`hasOwnProperty()` 메소드를 사용하면 프로퍼티가 인스턴스에 존재하는지 프로토타입에 존재하는 확인할 수 있다. 해당 프로퍼티가 객체 인스턴스에 존재할때만 true 반환

```javascript
function Person() {}

Person.prototype.name = "Nick";
Person.prototype.age = 20;
Person.prototype.job = "Sofrware Engineer";
Person.prototype.satName = function() {
	console.log(this.name);
}

const person1 = new Person()
const person2 = new Person()

console.log(person1.hasOwnProperty("name")); // false
console.log(person1.name) // Nick
console.log(person2.name) // Nick

person1.name = "Kang";
console.log(person1.hasOwnProperty("name")) // true

console.log(person1.name) // Kang
console.log(person2.name) // Nick

// delete 사용
delete person1.name;
console.log(person1.name) // Nick
console.log(person1.hasOwnProperty("name")); // false
```

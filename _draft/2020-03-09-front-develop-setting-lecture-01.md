---
title: "인프런 - 프론트엔드 개발환경의 이해와 실습"
categories: 
  - study
tags: 
    - lecture
    - online
    - inflearn
    - front
    - webpack
    - babel
    - npm
    - study
toc: true
toc_sticky: true
comments:  true
---

프론트개발의 프레임워크들은 다양한 스타터팩을 `github` 에서 찾아 볼 수가 있다. 이것들을 사용하면 굉장히 빠르고 편하게 개발환경의 세팅이 가능하다.  
 
이 도구들은 빠르게 화면의 결과물을 확인 할 수 있게 해주고 이로인해 `개발은 쉽다`, 또는 `진입장벽이 낮다` 라고 인지되어지고 있는 부분들이 많다.  

개인적으로는 반은 맞는말이지만 반은 틀렸다고 생각한다. 물론 입문하기에 있어 갈수록 개발이라는것이 일반인도 쉽게 접근이 가능하고 사용성이 편리해지는 방향성으로 나아가고있지만 역시 실무에서는 단순히 스타터팩으로 시작해서 그대로 진행되어지지 않는다. 

각자의 현재 상황과 조건에 맞춰서 그리고 추가되는 요구사항과 개발적인 특성 그리고 개발자들간의 문화와 성격에 따라 얼마든지 변경되고 `커스터마이징` 될 수 있다.  
  
나도 최근 1년간 이 회사에 입사해서 부분 부분 끄적이며 개발환경에 대해서 수정을 조금씩 진행하긴했지만 막상 처음부터 끝까지 환경 세팅을 진행하라고 하면 아마 어려움을 좀 겪지 않을까 싶다.  

이번에 인프런에서 아주 저렴한 가격에 이 주제를 가지고 강의를 한 내용이 있어서 나도 이번기회에 프론트개발환경에 대해서 좀 제대로 정리하고 최종적으로는 나도 스타터팩을 한번 만들어 보는것이 목표라고 할 수 있겠다.  


## 1. Node.js?
[공식문서](https://nodejs.org/ko/about/)  
  
**`Node.js` 는 `Chrome V8 JavaScript 엔진`으로 빌드된 `JavaScript` 런타임**
```
비동기 이벤트 주도 JavaScript 런타임으로써 Node.js 는 확장성있는 네트워크 애플리케이션을 만들 수 있도록 설계되었습니다.

(중략)

오늘날 OS 스레드가 일반적으로 사용하는 동시성 모델과는 대조적입니다. 스레드기반의 네트워크는 상대적으로 비효율적이고 사용하기가 몹시 어렵습니다.

(중략)

Node.js 에서 I/O(Input / Output)를 직접 수행하는 함수는 거의 없으므로 프로세스는 블로킹되지 않습니다. 아무것도 블로킹되지않으므로 확장성있는 시스템 개발하는게 아주 자연스럽다.
```  
`블로킹` : 어떠한 프로세스에서 하나의 작업 실행을 위해서는 이전에 실행된 작업이 완료될때까지 기다려야 하는 상황

``` javascript
// 동기로 파일을 읽는 예제
const fs = require('fs');
const data = fs.readFileSync('file.md'); 
console.log(data); // 추가작업은 console.log 이후 실행

// 비동기 예제
const fs = require('fs');
fs.readFile('/file.md', (err, data) => {
    if (err) throw err;
    console.log(data)
})
// 추가 작업은 console.log 이전에 실행 가능
```

## 2. npm
`npm` 은 `JavaScript` 프로그래밍 언어를 위한 `package` 관리자로 전세계의 다양한 개발자들이 자유롭게 자신만의 라이브러리나 패키지를 업데이트하고 다운로드하여 사용할 수 있게 되어있다.  

Node.js 를 설치하게되면 npm 은 자동으로 함께 설치된다.   
  
#### Node.js Download => npm init
``` json
// package.json
{
  "name": "frontdevelopsetting",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "echo \"여기에 빌드 스크립트 추가\""
  },
  "author": "",
  "license": "ISC"
}
```
``` javascript
npm test // Error: no test specified
npm run build // 여기에 빌드 스크립트 추가
```  

즉 scripts 에는 다양한 npm 관련 명령어 추가가 가능하다.
  
### npm install *
npm 패키지는 `npm install libraryName` 을 통해서 설치가 가능하다.
``` json
//  npm install react
{
  ...
  ...

  "dependencies": {
    "react": "^16.13.0"
  }
}
```

`npm install react` 를 한 이후 `package.json` 에 추가된 내용이다.  
  
### Sementic version
>"react": "^16.13.0"  

`Major Version - 16` : 기존 버전과 호환되지 않게 변경한 경우    
`Monor Version - 13` : 기존 버전과 호환되면서 기능이 추가된경우  
`Patch Version - 0` : 기존 버전과 호환되면서 버그를 수정한 경우  

`~(틸트)` : 마이너 버전이 명시되어 있으면 패치버전을 변경  
``` javascript
~1.2.3 // 1.2.3 부터 1.3.0미만 까지 포함
~0 // 0.0.0 부터 1.0.0 미만까지 포함
```  
  
`^(캐럿)` : 정식버전에서 마이너와 패치 버전을 변경
``` javascript
^1.2.3 // 1.2.3 부터 2.0.0미만 까지 포함
^0 // 정식버전 미만인 0.x 버전은 패치만 갱신 0.0.0 부터 0.1.0 미만까지
```  
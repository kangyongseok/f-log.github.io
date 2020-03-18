---
title: "인프런 - 프론트엔드 개발환경의 이해와 실습-04"
categories: 
  - study
tags: 
    - lecture
    - online
    - inflearn
    - front
    - webpack
    - babel
    - lint
    - npm
    - study
    - plugin
toc: true
toc_sticky: true
comments:  true
---

## Babel

`ES2015+` 의 문법으로 작성된 자바스크립트 코드를 모든 브라우저에서 동작할 수 있도록 변환해주는 역할을 한다. 즉 하위브라우저나 최신문법을 지원하지않는 브라우저에서도 내가 작성한 코드가 동작할 수 있는 하위코드로 자동으로 변환해준다고 볼 수 있겠다.  

## Babel install
``` javascript
$ npm install @babel/core @babel/cli

// 실행명령어
$ npx babel app.js
```

바벨은 세단계로 빌드를 진행
- 파싱
- 변환
- 출력


## Babel 사용법과 웹팩 통합
``` javascript
$ npm i @babel/preset-nev

// babel.config.js
module.exports = {
    presets: [
        '@babel/preset-env'
    ]
}

// 타겟 브라우저
module.exports = {
    presets: [
        ['@babel/preset-env', {
            targets: {
                chrome: '79',
                ie: '11'
            }
        }]
    ]
}

// 문법변환이 해당되지 않는 코드일때는 폴리필이 필요
module.exports = {
    presets: [
        ['@babel/preset-env', {
            targets: {
                chrome: '79',
                ie: '11'
            },
            useBuiltIns: 'usage', // 'entry', false
            corejs: {
                version: 2, // 3
            }
        }]
    ]
}
```


``` javascript
$ npx babel app.js

// 바벨로 변환된 문법
"use strict";

require("core-js/modules/es6.promise"); // 추가됨

require("core-js/modules/es6.object.to-string"); // 추가됨

new Promise();
```

## 웹팩으로 통합

``` javascript
$ npm i -D babel-loader
```

<!-- 커밋을 위한 주석 추가....... -->

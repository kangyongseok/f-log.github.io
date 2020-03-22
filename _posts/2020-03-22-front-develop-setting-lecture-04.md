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
// 바벨에서 목적에 따라 제공하는 프리셋

$ preset-env // 과거에 버전별로 바벨 설치가 달랐지만 env 하나로 통합됨
$ preset-flow // flow 를 변환하기 위한 프리셋
$ preset-react // React 를 변환하기 위한 프리셋
$ preset-typescript // typescript 를 변환하기 위한 프리셋

$ npm i @babel/preset-env

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

// webpack.config.js
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader',
            }
        ]
    }
}
```

## 정리
- 바벨의 코어는 파싱과 출력만 담당하고 변환 작업은 플러그인이 처리한다.
- 여러개의 플러그인들을 모아놓은 세트를 프리셋 이라고하는데 env 프리셋을 사용한다.
- 바벨이 변환하지 못하는 코드는 폴리필이라 부르는 코드조각을 불러와 결과물에 로딩해서 해결한다.


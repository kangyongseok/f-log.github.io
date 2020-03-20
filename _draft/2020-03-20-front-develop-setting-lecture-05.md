---
title: "인프런 - 프론트엔드 개발환경의 이해와 실습-05"
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

## 린트 (Lint)
- 포맷팅 (코드의 일관성)
- 코드품질
  
### 설치

``` javascript
$ npm install eslint --save-dev
$ npx eslint --init // .eslintrc 디렉토리에 파일이 생성
// 대화형 세팅 진행
```

``` javascript
// .eslintrc.js
module.exports = {
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": "eslint:recommended",
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "rules": {
    }
};
```
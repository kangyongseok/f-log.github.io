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

## Prettier
코드를 더 예쁘게 만들어 준다. 일관성있는 코드 스타일로 다듬어준다.

``` javascript
$ npm i -D prettier
$ npx eslint app.js --fix

```

실행하게되면 일부 `lint` 와 겹치는 부분이 있기때문에 중복되는 부분을 제거해서 통합하여 사용할 수 있게 만들어 주는 방법이 있다.

``` javascript
$ npm i -D eslint-config-prettier
$ npx prettier app.js --write

// .eslintrc.js
{
    extends: [
        "eslint:recommended",
        "eslint-config-prettier" // 추가된 내용
    ]
}
```

매번 두번씩 실행시키는것도 불편하다 한번에 실행가능하게 변경 가능하다

``` javascript
$ npm i -D eslint-plugin-prettier

// .eslintrv.js
{
    ...
    plugins: [
        "prettier"
    ],
    "rules": {
        "prettier/prettier": "error"
    }
}

$ npx exlint app.js --fix // 이 명령어 하나면 한번에 적용 가능
```
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

## 자동화
매번 코딩할때마다 `npm run` 을 통해서 린트를 실행하기에는 번거롭다 이를 자동화하는 방법이 있다. 깃 훅을 사용하면 커밋전 또는 푸시전 등 깃 명령어 실행 시점에 끼어들 수 있는 훅을 제공할 수 있다.
``` javascript
$ npm i -D husky

// package.json
{
    "husky": {
        "hooks": {
            "pre-commit": "eslint app.js --fix"
        }
    }
}
```

위와같이 husky 를 설치하고 package.json 을 설정하면 커밋전에 린트검사를 진행하고 에러가 발생하면 커밋을 진행하지 않게된다.  

그러나 프로젝트의 규모가 커질수록 린트의 검사속도도 증가하기때문에 모든 파일이 아니라 커밋이 진행된 파일에 대해서만 린트를 검사해줄 필요가 있다. 

``` javascript
$ npm i -D lint-staged

// package.json
{
    "husky": {
        "hooks": {
            "pre-commit": "lint-staged"
        }
    },
    "lint-staged": {
        "*.js": "eslint --fix"
    }
}
```
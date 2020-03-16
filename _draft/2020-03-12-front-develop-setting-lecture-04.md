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




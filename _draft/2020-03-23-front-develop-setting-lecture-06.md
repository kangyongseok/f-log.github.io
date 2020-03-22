---
title: "인프런 - 프론트엔드 개발환경의 이해와 실습-06"
categories: 
  - study
tags: 
    - lecture
    - online
    - inflearn
    - front
    - webpack
    - npm
    - study
    - plugin
    - 웹팩 심화
toc: true
toc_sticky: true
comments:  true
---

## 웹팩 심화


### 웹팩 개발 서버
프론트엔드 개발환경에서 개발용 서버를 제공하는것이 `webpack-dev-derver`

``` javascript
$ npm i -D webpack-dev-server

// package.json
{
    ...
    "scripts": {
        "start": "webpack-dev-server --progress"
    }
}

$ npm start
```

### 기본설정
웹팩 설정파일의 devServer 객체에 개발 서버 옵션을 설정할 수 있다.
``` javascript
// webpack.config.js
module.exports = {
    devServer: {
        contentBase: path.join(__dirname, "dist"), 
        publicPath: "/",
        host: "dev.domain.com",
        overlay: true,
        port: 8081,
        stats: "errors-only",
        historyApiFallback: true,
    }
}
```
`contentBase`: 정적파일을 제공하는 경로. 기본값은 웹팩 아웃풋이다.  
`publicPath`: 브라우저를 통해 접근하는 경로  
`host`: 개발환경에서 도메인을 맞추어야 하는 상황에서 사용  
`overlay`: 빌드시 에러나 경고를 브라우저 화면에 표시  
`port`: 개발 서버 포트 번호를 설정. 기본값은 8080  
`stats`: 메세지 수준을 설정. none, errors-only, minimal, normal, verbose  
`historyApiFallback`: 히스토리 API 를 사용하는 SPA 개발시 설정. 404가 발생하면 index.html로 리다이렉트 한다.  
  
개발서버 실행시 `--progress` 를 추가하면 빌드 진행을 보여준다.
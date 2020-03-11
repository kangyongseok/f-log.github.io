---
title: "인프런 - 프론트엔드 개발환경의 이해와 실습-02"
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
toc: true
toc_sticky: true
comments:  true
---

## Webpack
[공식문서](https://webpack.js.org/concepts/)  

웹팩 처음 화면에 `Write Your Code` 과 `Bundle It` 에 적혀있는 코드가 있는데 이게 웹팩의 모든것이 아닐까 싶다.  

**웹팩 빌드전 사용한 코드**  
``` javascript
// src/index.js
import bar from './bar';

bar()

// src/bar.js
export default function bar() {
    //
}
```    
  

**웹팩 번들후 변환된 코드**  
``` javascript
// webpack.config.js
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    }
}

// page.html
<!doctype html>
<html>
    <head>
        ...
    </head>
    <body>
        ...
        <script src="dist/bundle.js"></script>
    </body>
</html>
``` 
  
웹팩이 없이 기존에 쓰던 방식을 보면   
``` javascript
// math.js
function sum(a, b) {
    return a + b;
}

// app.js
console.log(sum(1, 2)) // 3
```

``` html
// index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <script src="src/math.js"></script>
    <script src="src/app.js"></script>
</body>
</html>
```
  
이렇게 사용했을경우 문제되는 부분은 `sum` 이라는 함수는 현재 전역으로 선언되어있고 `javascript` 는 충분히 저 전역 함수를 오염시켜 에러가 발생하는경우가 생길수밖에 없다.
  
이러한 문제를 해결하기 위한 방법으로 `IIFE(즉시실행함수)` 라는 방법을 사용할 수 있다.
``` javascript
// math.js
var math = math || {}; // math 가 있으면 math 없으면 빈객체

// 즉시실행함수 스코프 내부에 sum 함수를 작성하고
(function() {
    function sum(a, b) {
        return a + b;
    }
    // 전역 변수인 math 에 sum 이라는 객체를 추가하고 함수를 넘겨준다.
    math.sum = sum;
})();

// app.js 에서는 math 라는 전역변수를 사용해야만 sum 이라는 함수를 사용할 수 있다.
console.log(math.sum(1, 2)); // 3
```

이후 여러가지 모듈을 다루는 방식들이 나오다가 `ES2015` 에서 표준 모듈시스템을 만들게되고 이게 바로 지금 내가 `react` 에서도 다른 라이브러리를 사용하면서도 자주쓰는 방식이 나오게 된것같다.  

``` javascript
// math.js
export function sum(a, b) { return a + b; }

// app.js
import * as math from './math.js';
math.sum(1, 2); // 3
```

그러나 모든 브라우저에서 이 모듈 사용방식을 지원하는것이 아니라는 단점이 있다. 그래서 이 부분을 자동화 하기위해 나온것이 웹팩이라고 볼 수 있겠다.  

크롬이 아닌 다른 브라우저에서도 모듈시스템을 사용 할 수 있도록 번들을 통해 제일 처음에 나왔던 코드처럼 변환을 해준다고 볼 수 있을것같다.
  
모듈로 개발을 하다보면 무수히 많은 모듈이 생기고 그 모듈들간에 의존성도 생기게된다. 이러한 의존성을 가진 `.js` 파일들을 하나의 `.js` 로 합쳐주는것이 웹팩이 해주는 역할이라고 할 수 있다.

## Webpack 실습
``` javascript
// webpack 과 webpack-cli 를 -D 옵션으로 개발용 패키지로 설치
$ npm install -D webpack webpack-cli
```

``` json
// package.json
  "dependencies": {},
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11"
  }
```

웹팩을 실행하기 위해서는 `Config options` 중에 `--mode` 를 입력해야하는데 세가지 옵션이 있다.   
`development` : 개발전용으로 실행할때 사용하는 옵션,   
`production` : 실제 서버에 배포할때 사용하는 옵션,   
`none`
  
`Basic option` 중에는  
`--entry` : 모듈의 시작점 지점  

`Output option`  
`--output` : 저장할 지점

``` javascript
// mode: development
// entry: ./src/app.js
// output: dist/main.js 
$ node_modules/.bin/webpack --mode development --entry ./src/app.js --output dist/main.js

// console
Hash: 6667e6ce7ba6b7c8991f
Version: webpack 4.42.0
Time: 44ms
Built at: 2020. 03. 11. 오전 5:38:01
  Asset      Size  Chunks             Chunk Names
main.js  3.79 KiB    null  [emitted]  null
Entrypoint null = main.js
[./src/app.js] 28 bytes {null} [built]
```

이렇게 실행하면 `dist/main.js` 라는 티렉토리가 하나 생성되고 
``` html
<body>
    <script type="module" src="dist/main.js"></script>
</body>
```
스크립트파일도 이렇게 하나만 불러오면 된다.  

웹팩을 실행하기위한 명령어 옵션은 저것뿐만아니라 더 늘어나고 또 환경별로 다르게 설정되어지는 부분이 있는데 그럴떄마다 매번 저렇게 긴 옵션을 입력하면서 실행할 수 없기때문에 이것을 설정하기위한 `webpack.config.js` 파일을 생성하고 여기에 웹팩 실행 옵션들을 설정한다.

``` javascript
const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        main: './src/app.js' // 의존관계가 있는 모듈의 시작점
    },
    output: { // 번들링한 결과를 output option 에 따라 생성
        path: path.resolve('./dist'),
        filename: '[name].js' // entry 가 여러개일수 있어서 이렇게 설정
    },
}
```

그리고 웹팩을 실행하기위한 명령어를 `package.json` 에 입력해준다.
``` json
"scripts": {
    ...
    "build": "webpack"
},
```
``` javascript
// 실행
$ npm run build
```

## 로더
웹팩은 모든 파일은 모듈로 바라본다. `자바스크립트`를 포함 `css, font, img(jpg/png/gif..)` 까지 모듈로 보기때문에 `import`를 통해서 자바스크립트 코드안으로 가져올 수 있다.  

이것을 가능하게 하는게 바로 로더이다.

**my-webpack-laode.jsr**
``` javascript
module.exports = function myWebpackLoader (content) {
    return content.replace('console.log(', 'alert(');
}
```

**webpack.congif.js**

``` javascript
module: {
    rules: [
        {
            test: /\.js$/, // .js 확장자로 끝나는 모든 파일
            use: [path.resolve('./my-webpack-loader.js')] // my-webpack-loader.js 를 적용
        }
    ]
}
```

위처럼 작성하게되면 `.js` 로 끝나는 파일들에 `console.log()` 들을 `alert()` 으로 변경해서 빌드하는것이 가능해진다.
  

## CSS Loader
  
``` javascript
$ npm install css-loader
```

**webpack.config.js**
``` javascript
...
module: {
    rules: [
        {
            test: /\.css$/,
            use: [
                'css-loader'
            ]
        }
    ]
}
```

**src/app.css**
``` css
body {
    background: red;
}
```

이렇게 작성하고 `$ npm run build` 진행하면 main.js 내부에 css 가 들어가있는걸 볼 수 있지만 index.html 을 실행하고 개발자 도구를보면 아직 html 에 css가 들어가있지는 않다. 이걸 가능하게 하려면 `$ npm install style-loader` 를 설치해야한다.

**webpack.config.js**
``` javascript
...
module: {
    rules: [
        {
            test: /\.css$/,
            use: [
                // loader 는 여러개 추가할 수 있는데 뒤에서부터 실행
                'style-loader',
                'css-loader'
            ]
        }
    ]
}
```

## Image Laoder

``` javascript
$ npm install file-loader
```

**src/app.css**
``` css
body {
    background: url('test.png');
}
```

**webpack.config.js**
``` javascript
module: {
    rules: [
        ...
        {
            test: /\.png$/,
            use: [
                'file-loader',
            ]
        }
    ]
}
```

`$ npm run build` => `dist/` 에 해싱으로 파싱된 파일명을 가진 이미지 파일이 하나 생성된다.  

`index.html` 을 실행하여 웹을 열어보면 이미지가 아직 적용이 안된다. 바로 `css` 에적용한 `image` 루트가 잘못되어있기 때문이다.

**webpack.config.js**
``` javascript
...
{
    test: /\.png$/,
    loader: 'file-loader',
    options: {
        /*
        [name] : file-loader 가 output 에 복사할때 쓰는 파일명
        [ext] : 확장자명
        [hash] : 매번 달라지는 hash 값 입력
        */
        publicPath: './dist', // output 폴더명 path를 설정해준다.
        name: '[name].[ext]?[hash]'
    }
}
```

## URL Loader
사용하는 이미지 갯수가 많아지면 네트워크 리소스를 사용하는데 부담이 생기고 성능에 영향을 미칠 수 있다.  

한페이지에서 작은 여러 이미지를 사용한다면 Data URI Scheme 를 이용하는게 더 낫다. 이미지를 Base64 로 인코딩하여 문자열 형태로 소스코드에 넣는 형식인데 url-loader 는 이 처리를 자동화해준다.

``` javascript
$ npm install -D url-loader
```

**webpack.config.js**
``` javascript
// 20kb 이하의 이미지파일은 base64 로 변환하여 <img src="" /> 에 들어가고 그 이상의 파일들은 복사를통해 './dist' 폴더안에 들어가게 된다.
...
{
    test: /\.(png|jpg|gif|svg)$/,
    loader: 'url-loader',
    options: {
        publicPath: './dist',
        name: '[name].[ext]?[hash]',
        limit: 20000, // 20kb
    }
}
```
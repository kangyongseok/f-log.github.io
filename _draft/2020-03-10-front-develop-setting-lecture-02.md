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
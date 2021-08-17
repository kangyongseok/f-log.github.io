---
title: "프로그래머스 과제관 - 고양이 사진 검색 사이트"
categories: 
  - javascript
tags: 
    - 자바스크립트
    - 프로그래머스
    - 과제관
    - 프론트엔드 과제
toc: true
toc_sticky: true
comments:  true
---

## 과제 구조 파악
```bash
index.html
src/main.js
src/App.js
src/api.js
src/ImageInfo.js
src/SearchInput.js
src/SearchResult.js
src/utils/validator.js
```
일단 html 이 하나인 원 페이지로 구성되어있습니다. 라이브러리나 어떤 도구도 사용할 수 없고 순수 JS 로만 문제 해결을 하면서 구현해 나가야 하는 과제입니다. 
  
해당 과제는 어느정도 프로젝트의 틀이 잡혀있는 형태고 코드의 구조도 클래스형태로 이미 짜여져 있는 상태에서 수정과 기능 추가를 진행해야 하기때문에 어느정도 세팅되어있는 코드의 흐름이나 구조 그리고 자바스크립트 클래스문법에 대한 이해를 필요로합니다.

### index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="src/style.css" />
    <title>Cat Search</title>
  </head>
  <body>
    <div id="App"></div>
    <script src="src/utils/validator.js"></script>
    <script src="src/api.js"></script>
    <script src="src/ImageInfo.js"></script>
    <script src="src/SearchInput.js"></script>
    <script src="src/SearchResult.js"></script>
    <script src="src/App.js"></script>
    <script src="src/main.js"></script>
  </body>
</html>
```
- ES6 module 형태로 코드를 변경합니다.

현재는 html 내에서 모든 스크립트 파일을 불러오고있는데 요구사항을 반영하고나면 하나의 스크립트만 사용되고있어야하고 각 스크립트 파일들도 ES6 모듈형태로 import 와 export 로 수정해야합니다.

```html
<!-- index.html -->
 <body>
    <div id="App"></div>
    <script type="module" src="src/main.js"></script>
  </body>
```

```js
// main.js
import App from './App.js'; // import 추가
new App(document.querySelector("#App"));
```

```js
// App.js
// import 추가 export default 로 App 클래스 내보내기
import SearchInput from './SearchInput.js';
import SearchResult from './SearchResult.js';
import ImageInfo from './ImageInfo.js'; // export 가 default 일떄
import { api } from './api.js'; // export 할 함수가 여러개일떄 { api, ... }

export default class App {
    ... // 생략
}
```

그리고 각각의 페이지 컴포넌트로 사용될 클래스객체들도 export default 를 선언해 줍니다.
```js
export default class ImageInfo {...}
export default class SearchInput {...}
export default class SearchResult  {...}
```

클래스 문법에 대해서는 조금 뒤에 하나씩 풀어서 보기로하고 우선 ES6 문법으로 변환을 마저 진행하겠습니다.

- API fetch 코드를 `async`, `await` 문을 이용하여 수정해주세요. 에러가 났을경우를 대비해 적절히 처리가 되어있어야 합니다.
- API 의 status code 에 따라 에러 메세지를 분리하여 작성해야 합니다. 
```js
// 예시
const request = async (url: string) => {
    try {
        const result = await fetch(url);
        return result.json();
    } catch(e) {
        console.wran(e);
    }
}

const api = {
    fetchGif: keyword => {
        return request( `${API_ENDPOINT}/api/gif/search?q=${keyword}` );
    },
    fetchGifAll: () => {
        return request( `${API_ENDPOINT}/api/gif/all` );
    }
};
```

```js
// api.js 수정전
const API_ENDPOINT =
    "https://oivhcpn8r9.execute-api.ap-northeast-2.amazonaws.com/dev";

export const api = {
    fetchCats: keyword => {
        return fetch(`${API_ENDPOINT}/api/cats/search?q=${keyword}`).then(res =>
            res.json()
        );
    }
};

```
이건 예시대로 수정하면 될것같습니다.

```js
// api.js 수정후
const request = async (url) => {
    // try~catch 문으로 try 내에 작성된 코드에서 에러가 발생하면 catch 에서 에러를 전달받아 내부에서 처리합니다. 
    // request 는 api 파일 내에서만 사용하기때문에 export 하지 않아도 됩니다.
    try {
        const result = await fetch(url);
        return result.json();
    } catch(e) {
        console.wran(e);
    }
}

export const api = {
    fetchCats: keyword => {
        return request( `${API_ENDPOINT}/api/cats/search?q=${keyword}` );
    },
};
```

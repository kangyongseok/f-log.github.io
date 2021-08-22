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

일단 구조관련해서 어느정도 수정사항은 진행했습니다. 나머지 수정사항을 진행하기전에 어떤 코드구조를 갖고있고 어떻게 읽어나가서 어느부분을 수정하면 되는지 알아보도록 하겠습니다.

## 코드 구조 파악
```js
// main.js
import App from './App.js';
new App(document.querySelector("#App"));
```
`main.js` 는 자바스크립트의 제일 처음 진입점입니다. 때문에 html 에서도 main.js 만 스크립트로 불러왔고 나머지 파일은 제거했습니다. main.js 에서 App.js 를 불러와 클래스로 선언된 App 객체를 new 식별자로 호출하고 html 에 있던 `id="App"` 을 선택해서 매개변수로 넘겨줍니다.

### 자바스크립트 Class 문법
자바스크립트에서 Class 는 ES6 에서 도입된 문법으로 원래는 없던 문법입니다. 자바나 다른 객체지향언어에 있던 클래스 문법을 비슷하게 흉내냈다고 볼 수 있는데요 자바스크립트에서는 이 클래스문법또한 하나의 객체이자 함수라고 볼 수 있습니다.
  
다만 new 라는 식별자를 무조건 붙여서 호출해야하고 Class 문법일때만 갖는 특징이 몇가지 있어서 일반적인 함수 호출과는 차별점을 두고 사용합니다.

```js
class ClassTest {} // class 식별자로 선언
new ClassTest() // 인스턴스 생성
```
위와같이 선언하고 호출(인스턴스 생성)하여 사용하면 됩니다. class 만 갖는 특징을 확인해보겠습니다.
  
class 식별자를 사용하고있지만 클래스만의 특징을 내부적으로 갖고있지만 호출방식을 보면 함수입니다. 때문에 호출하면서 매개변수를 넘겨줄수도있고 클래스내에서는 `constructor` 로 전달받아 초기화를 시킬 수 있습니다.

```js
class ClassTest {
    a = 1;
    b = 2;
}
new ClassTest() // {a: 1, b: 2} // 인스턴스 생성
```

class 로 선언한 함수를 호출하게되면 객체가 생성됩니다. 함수처럼 매개변수로 값을 넘겨주는건 constructor 로 받아서 사용할 수 있습니다.

```js
class ClassTest {
    constructor(target) {
        this.target = target
    }
}
new ClassTest('target') // {target: "target"}
```

`this.target` 에서 this 는 생성한 객체 자기 자신을 가리킵니다. `ClassTest` 에 target 이란 키로 넘어온 매개변수값을 할당하겠다고 초기화 시키는 작업을 여기서 진행합니다. `constructor` 는 생성자라고 부르고 하나의 클래스에 하나의 constructor 만 사용할 수 있습니다.
  
클래스내에서는 static 이라는 키워드로 정적 속성을 만들어 줄 수 있는데 이 정적속성으로 만들어진 객체는 인스턴스를 생성하여 사용이 불가능합니다. 

```js
class ClassTest {
    static name = "홍길동"
}

const test = new ClassTest(); // 인스턴스 생성
test.name // undefined
ClassTest.name // 홍길동
```

### class App 분석
```js

export default class App {
    $target = null;
    data = [];

    constructor($target) {
        this.$target = $target;

        this.searchInput = new SearchInput({
            $target,
            onSearch: keyword => {
                api.fetchCats(keyword).then(({ data }) => this.setState(data));
            }
        });

        this.searchResult = new SearchResult({
            $target,
            initialData: this.data,
            onClick: image => {
                this.imageInfo.setState({
                    visible: true,
                    image
                });
            }
        });

        this.imageInfo = new ImageInfo({
            $target,
            data: {
                visible: false,
                image: null
            }
        });
    }

    setState(nextData) {
        console.log(this);
        this.data = nextData;
        this.searchResult.setState(nextData);
    }
}

```
긴 코드에 겁먹지말고 하나씩 보도록하겠습니다.

```js
export default class App {
    $target = null;
    data = [];
    ...
}
```
위에 저 두값은 뭐냐면 public 필드에 선언된 초기화된 변수라고 보면됩니다. 인스턴스를 생성해보면
```js
{ $target: null, data: Array(0) }
```
이렇게 객체 형태로 만들어 집니다. App 클래슨 내부에 다른 컴포넌트들이 인스턴스호출되어있는데 하위컴포넌트 즉 클래스객체에도 공통으로 사용될 전역변수같은거라고 보시면 됩니다.

```js
constructor($target) {
    this.$target = $target;

    this.searchInput = new SearchInput({
        $target,
        onSearch: keyword => {
            api.fetchCats(keyword).then(({ data }) => this.setState(data));
        }
    });

    this.searchResult = new SearchResult({
        $target,
        initialData: this.data,
        onClick: image => {
            this.imageInfo.setState({
                visible: true,
                image
            });
        }
    });

    this.imageInfo = new ImageInfo({
        $target,
        data: {
            visible: false,
            image: null
        }
    });
}
```
`constructor` 가 꽤 긴데 main.js 에서 넘겨주고있는 `document.querySelector("#App")` 을 `$target` 이란 이름으로 받아서 this.$target 에 초기화 해 주었습니다.
  
그리고 각각 페이지의 컴포넌트를 담당할 인스턴스들을 생성하고 초기화 하였습니다.
```js
this.searchInput = new SearchInput()
this.searchResult = new SearchResult()
this.imageInfo = new ImageInfo()
```
생성한 인스턴스로 필요한 데이터객체들을 넘겨주고있습니다.


```js
this.searchInput = new SearchInput({
    $target,
    onSearch: keyword => {
        api.fetchCats(keyword).then(({ data }) => this.setState(data));
    }
});
```
SearchInput 에 public 에 선언하고 초기화한 $target 객체와 onSearch 라는 함수객체를 전달합니다.

```js
 this.searchResult = new SearchResult({
    $target,
    initialData: this.data,
    onClick: image => {
        this.imageInfo.setState({
            visible: true,
            image
        });
    }
});
```

SearchResult 에도 $target 과 public 에 선언한 data 를 전달하고 onClick 함수객체를 넘겨주고 있습니다. 각 인스턴스를 생성하면서 넘겨주는 메소드들은 매개변수로 리턴값을 전달받아 처리하고 있습니다. `SearchResult` 는 매개변수를 전달받아 `imageInfo` 에 setState 함수를 호출하면서 객체를 전달합니다.

### class SearchInput 분석
```js
export default class SearchInput {

  constructor({ $target, onSearch }) {

    const $searchInput = document.createElement("input");

    this.$searchInput = $searchInput;
    this.$searchInput.placeholder = "고양이를 검색해보세요.|";

    $searchInput.className = "SearchInput";
    $target.appendChild($searchInput);

    $searchInput.addEventListener("keyup", e => {
      if (e.keyCode === 13) {
        onSearch(e.target.value);
      }
    });

    console.log("SearchInput created.", this);
  }

  render() {}
}
```
`SearchInput` 내부에는 크게는 `constructor`와 render 메소드가 있습니다. `constructor` 에는 App.js 에서 전달해주고있는 매개변수 객체를 받았습니다. 
  
this.$searchInput 에 생성한 input element를 바인딩하여 초기화 시키고 placeholder, classname, event 를 지정해 주었습니다. 여기서 plcaholder 는 this.$searchInput.placeholder 로 this 를 붙여서 할당해줬는데 사실 this 는 더이상 붙여주지 않아도 됩니다.
  
`$target` 은 main.js 에서부터 넘어온 `<div id="App"></div>` 을 받아왔고 여기에 `appendChild` 로 자식요소로 생성한 input을 추가합니다. 이 코드로 인해서 화면에 input 입력창이 보여지게되고 event 바인딩으로 인해 keycode === 13 은 enter 키의 키코드이기때문에 enter를 입력했을때 onSearch 이벤트가 실행됩니다.



### class SearchResult 분석
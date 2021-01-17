---
title: "모바일의 브라우저별 input type=file 처리방법"
categories: 
  - frontend
tags: 
    - os
    - mobile
    - in-app browser
    - input
    - file
    - accept
toc: true
toc_sticky: true
comments:  true
---

## Mission
리뷰를 작성하는 화면을 개발하는걸 맡았다. 카카오 알림톡으로 링크가 전달되고 그 링크로 접근했을때 리뷰를 작성하는 카카오 인앱 브라우저가 띄워지면서 리뷰를 작성해 나가는 방식이다.  
  
리뷰를 작성해나가며 각 스텝이 존재하고 첫번째 스텝에서는 사진첨부 기능이 추가되어야 한다. (이게 지옥의 시작)
  
일반적으로 파일 첨부 기능을 추가하기위해 input 을 사용한다.
```html
<input
    type="file"
    accept="image/*"
/>
```
`accept="image/*"` 를 사용하면 어떤 파일을 업로드할지 지정할 수 있다. 이번에 진행한 프로젝트는 이미지 첨부가 필요했고 전체 이미지 확장자를 다 허용하도록 accept 를 작성해 주었다.
  
## 문제점
일단 고려해야할 부분은 리뷰작성은 모바일에서 진행하게 되는데 브라우저는 카카오알림톡으로 가다보니 카카오인앱브라우저에서 작성을 진행하게된다. 단순히 리뷰 내용을 선택하고 작성만 하는거면 문제가 없었겠지만 사진첨부가 포함되다보니 모바일 OS 의 서로 다른 동작과 카카오 인앱브라우저이기때문에 일반 브라우저와 다르게 동작되는 부분에 대한 이해가 필요했다.

- IOS, Android 의 차이
- 일반브라우저가 아닌 카카오인앱브라우저에서 동작

일단 IOS 는 카카오인앱브라우저에서도 사진첨부가 카메라와 갤러리접근이 모두 가능해서 전혀 문제될게 없었다.
  
문제는 안드로이드, 원래대로라면 `accept="image/*"` 이 부분만 있다면 하단에 카메라와 갤러리중 선택할 수 있도록 나와야한다. 그러나 안드로이드 9버전 이하에서는 카메라와 폴더만 나오는 이슈가 있다.
  
사용자들을 확인한결과 대부분 다행이 9이상의 버전을 사용중인걸로 확인이되었다. 문제는 카카오인앱브라우저에서 안드로이드의 버전에 상관없이 무조건 카메라와 폴더가 나온다는것이다. 즉 갤러리에 바로 접근이 불가능하다.
  
카카오인앱브라우저 가 아닌 브라우저라면 정상적으로 갤러리가 나오고 접근이 가능한데 여기만 안된다. 당장 다음주에 배포해야하는 상황인데 해결책이 필요했다. input 에 accept 와 capture 속성을 이용해서 다양하게 시도해봤는데 카카오인앱브라우저에서는 아무것도 안된다.
  
[캡쳐속성 테스트](https://addpipe.com/html-media-capture-demo/)
  
위 사이트에 들어가면 이미 작성된 예제로 파일첨부 테스트가 가능한데 바로 카메라가 나오도록하거나 오디오첨부 비디오 첨부가 가능하게 하는예제들도 나와있다.


## 해결

아무튼 해결책이 필요했는데 안드로이드일때 강제로 모바일에있는 크롬브라우저로 해당 페이지를 띄우는 방법이 있어 이걸 시도해보게되었다.

```javascript
location.href='intent://<url>#Intent;scheme=http;package=com.android.chrome;end'
```
위에서 `<url>` 부분만 크롬에서 새로 실행할 페이지 주소를 넣어주면된다. 자 이제 안드로이드에서 강제로 크롬을띄워서 카카오인앱브라우저의 단점에서 벗어났지만 해당 링크는 카카오인앱브라우저에서 안드로이드일때만 실행되어야하는 코드이고 만약 안드로이드이더라도 기타 다른 브라우저에서 리뷰작성페이지를 열었다면 기존과 동일하게 동작이 되어야만한다.

- 카카오인앱브라우저 + 안드로이드 => 크롬브라우저로 새창 띄우기
- 기타브라우저 + 안드로이드 => 현재 브라우저에서 진행

현재 브라우저가 카카오톡 브라우저인지 확인하기위해 `ua-parser-js` 를 사용하였다  

```javascript
var ua = new UAParser();
```

새 객체를 생성해주고 `ua.getUA()` 를 찍어보니 카카오톡인앱브라우저에서는 제일 마지막 부분에 KAKAOTALK 가 찍혀져 나오는걸 확인했고 해당 문자열을 체크함으로 인앱브라우저일때만 크롬으로 새창띄우는것이 가능해졌다.
  

## ...
처음 관련 작업 미팅을 할때 체크하고 시작해야하는 부분이었는데 아직도 개발자로서 많이 부족하다고 느낀다. 당연히 모바일 환경이라면 IOS 와 안드로이드의 서로 다른 부분이 있을거라는걸 체크못한게 실수였고 또 카카오인앱브라우저로 실행되기때문에 발생할 문제에 대해서도 무지했었던것같다.
  
대부분의 작업이 웹이면 웹 모바일이면 모바일 이렇게 분리된 형태로 작업을 해왔었는데 모바일에서 브라우저로 동작되야하고 OS 별로 다른부분이 충분히 존재하는 사진첨부 및 갤러리 접근에 대한 고려가 부족했었던것같다

하다못해 중간에 크로스체크만 제대로 하면서 작업을 진행했어도 지금보다는 빨리 발견하고 미리 조치를 취하거나 다른 기획의도로 접근할 시간이 있었을지도 모르겠다. 테스트는 여러사람이 다같이 진행하긴했지만 어쨋든 이런 크로스체크나 사전에 이런 사항들을 모르고 개발을 진행했다는것이 좀 부끄러운상황이 아닐까 하는 생각이 든다.

그래도 이번에 이런 경험을 토대로 다음번에 비슷한 상황이 발생했을때 충분히 사전에 이러한 부분을 체크하고 논의하고 개발을 진행 할 수 있도록 기억하고 주의하기위해 이렇게 블로그도 작성하니 절대 까먹지말자

>**프론트 개발자는 항상 크로스체크를 해야하고 다양한 환경에서 발생 할 수 있는 문제에 대해서 사전에 체크하고 확인하고 또 논의할 수 있어야 한다.**
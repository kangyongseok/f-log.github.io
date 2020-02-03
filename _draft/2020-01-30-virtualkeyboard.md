---
title: "Virtual Keyboard 제작부터 npm 배포까지"
categories: 
  - frontend
tags: 
    - VueJs
    - virtualkeyboard
    - npm
    - javascript
    - frontend
    - developer
toc: true
toc_sticky: true
comments:  true
---

2020년이 시작되면서 새롭게 맡게된 프로젝트가 키오스크의 제작이었다. 스펙은 윈도우기반이라 vue + electron 으로 제작하게되었다.  
  
태블릿모드로 사용하게되면 input 터치시 자동으로 window 의 가상키보드가 뜨게 되어있는데 데스크탑모드로 사용할 예정이기떄문에 가상키보드를 커스텀하여 사용하기로 하였다.  
  
큰 규모가 아니라 간단한 요청을 위한 제작이라 페이수나 기능이 많이 필요없었고 빠르게 개발하기위해서 가상키보드 라이브러리를 가져다 쓰려고했는데 한글지원하는건 찾았는데 자음과 모음이 분리입력되는 현상이 있어 사용할 수가 없었다.
  
방법을 찾아보던중 [hangul.js](https://github.com/e-/Hangul.js/) 라는 자모음 분리/조합을 해주는 라이브러리를 찾았고 이걸 활용해 가상키보드를 제작할 수 있었다.
  

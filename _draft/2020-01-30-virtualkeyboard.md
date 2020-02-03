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
  
## Setting
> vue-electron 세팅이 아닌 npm에 가상키보드를 배포가위한 세팅임을 미리 알립니다.

배포를위한 세팅은 [Vue 컴포넌트를 오픈소스로 npm에 배포하기](https://velog.io/@ashnamuh/Vue-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4%EB%A1%9C-npm%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0-dxjxfg5v7e) 라는 블로그 글을 참고하여 세팅하였고 이름은 [vue-virtual-keyboard-ko](https://www.npmjs.com/package/vue-virtual-keyboard-ko) 라고 지었다.
  
## Code
**src/components/VirtualKeyboard/keyData.js**
``` javascript
const kr = [
  [
    ['`', '~'], ['1', '!'], ['2', '@'], ['3', '#'], ['4', '$'], ['5', '%'], ['6', '^'], ['7', '&'], ['8', '*'], ['9', '('], ['0', ')'], ['-', '_'], ['=', '+'], ['BackSpace', 'BackSpace']
  ],
  [
    ['Tab', 'Tab'], ['ㅂ', 'ㅃ'], ['ㅈ', 'ㅉ'], ['ㄷ', 'ㄸ'], ['ㄱ', 'ㄲ'], ['ㅅ', 'ㅆ'], ['ㅛ', 'ㅛ'], ['ㅕ', 'ㅕ'], ['ㅑ', 'ㅑ'], ['ㅐ', 'ㅒ'], ['ㅔ', 'ㅖ'], ['[', '{'], [']', '}'], ['\\', '|']
  ],
  [
    ['CapsLock', 'CapsLock'], ['ㅁ', 'ㅁ'], ['ㄴ', 'ㄴ'], ['ㅇ', 'ㅇ'], ['ㄹ', 'ㄹ'], ['ㅎ', 'ㅎ'], ['ㅗ', 'ㅗ'], ['ㅓ', 'ㅓ'], ['ㅏ', 'ㅏ'], ['ㅣ', 'ㅣ'], [';', ':'], ['\'', '"'], ['Enter', 'Enter']
  ],
  [
    ['Shift', 'Shift'], ['ㅋ', 'ㅋ'], ['ㅌ', 'ㅌ'], ['ㅊ', 'ㅊ'], ['ㅍ', 'ㅍ'], ['ㅠ', 'ㅠ'], ['ㅜ', 'ㅜ'], ['ㅡ', 'ㅡ'], [',', '<'], ['.', '>'], ['/', '?'], ['한/영', '한/영']
  ],
  [
    ['space', 'space']
  ]
]

const en = [
  [
    ['`', '~'], ['1', '!'], ['2', '@'], ['3', '#'], ['4', '$'], ['5', '%'], ['6', '^'], ['7', '&'], ['8', '*'], ['9', '('], ['0', ')'], ['-', '_'], ['=', '+'], ['BackSpace', 'BackSpace']
  ],
  [
    ['Tab', 'Tab'], ['q', 'Q'], ['w', 'W'], ['e', 'E'], ['r', 'R'], ['t', 'T'], ['y', 'Y'], ['u', 'U'], ['i', 'I'], ['o', 'O'], ['p', 'P'], ['[', '{'], [']', '}'], ['\\', '|']
  ],
  [
    ['CapsLock', 'CapsLock'], ['a', 'A'], ['s', 'S'], ['d', 'D'], ['f', 'F'], ['g', 'G'], ['h', 'H'], ['j', 'J'], ['k', 'K'], ['l', 'L'], [';', ':'], ['\'', '"'], ['Enter', 'Enter']
  ],
  [
    ['Shift', 'Shift'], ['z', 'Z'], ['x', 'X'], ['c', 'C'], ['v', 'V'], ['b', 'B'], ['n', 'N'], ['m', 'M'], [',', '<'], ['.', '>'], ['/', '?'], ['한/영', '한/영']
  ],
  [
    ['space', 'space']
  ]
]

const num = [
  ['7', '8', '9'],
  ['4', '5', '6'],
  ['1', '2', '3'],
  ['', '0', '']
]

const email = []

export default ({
  kr,
  en,
  num,
  email
})
```
우선 키보드에 표시할 키 내용들을 배열데이터로 만들어두었다.   
지금보니 숫자키보드 값 부분은 중복되는 부분이니 별도로 변수로 뺴서 만들어두었어도 됬을것같다.
  
## Result
- [vue-virtual-keyboard-ko](https://www.npmjs.com/package/vue-virtual-keyboard-ko)
- [github repogitory](https://github.com/kangyongseok/vue-virtual-keyboard-ko)
  
## Reference
- [hangul.js](https://github.com/e-/Hangul.js/)
- [Vue 컴포넌트를 오픈소스로 npm에 배포하기](https://velog.io/@ashnamuh/Vue-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4%EB%A1%9C-npm%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0-dxjxfg5v7e)
- [Vue 컴포넌트를 npm에 등록하기](https://steemit.com/vue/@stepanowon/vue-npm)
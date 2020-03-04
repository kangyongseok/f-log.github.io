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

2020년이 시작되면서 새롭게 맡게된 프로젝트가 키오스크의 제작이었다. 스펙은 윈도우기반이라 `vue + electron` 으로 제작하게되었다.  
  
태블릿모드로 사용하게되면 input 터치시 자동으로 `window` 의 가상키보드가 뜨게 되어있는데 데스크탑모드로 사용할 예정이기때문에 가상키보드를 커스텀하여 사용하기로 하였다.  
  
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
  
  
  
**src/components/VirtualKeyboard/index.vue**  
``` javascript
import KeyData from './keyData'
import Hangul from 'hangul-js'
```
한글 자음과 모음을 합치고 분리해주는 라이브러리와 키보드 데이터를 import 하고
  
  
  
``` html
<template>
    <div class="keyboard">
      <div v-for="(keyLine, index) in KeyData[lang]" :key="index">
        <ul>
          <li
            v-for="(key, index) in keyLine"
            :key="index"
            class="key"
            v-bind:class="classObject(key[shiftIndex])"
            @click="() => keyEvent(key[shiftIndex])"
          >
            <span class="keyInfo" v-if="key[shiftIndex] === 'space'"> </span>
            <span class="keyInfo" v-else>{{ key[shiftIndex] }}</span>
          </li>
        </ul>
      </div>
    </div>
</template>
```
UI 를 담당할 template 코드도 작성한다.
  
  
  
``` html
  <div v-for="(keyLine, index) in KeyData[lang]" :key="index">
```
이 태그의 역할은 import 한 keyData 를 가져와 v-for 로 반복을 도는데 ```KeyData[lang]``` 를 해준이유는 한영키를 눌렀을때 한글 <==> 영어를 동적으로 보여주기 위해서다. ```:key="index" ``` 는 v-for 문을 작성하게되면 반드시 함께 포함되어야하는 속성이다.  
  
  
  
``` html
 <li
    v-for="(key, index) in keyLine"
    :key="index"
    class="key"
    v-bind:class="classObject(key[shiftIndex])"
    @click="() => keyEvent(key[shiftIndex])"
  >
```
div 에서 반복문으로 돌려 라인별로 데이터를 반복하고 그 안에있는 진짜 키보드가 li 태그에 담겨진다.  
```v-bind:class```  는 특수키들이 가질 동적 클래스명이된다.  
  
  
  
``` html
  <span class="keyInfo" v-if="key[shiftIndex] === 'space'"> </span>
  <span class="keyInfo" v-else>{{ key[shiftIndex] }}</span>
```
첫번째 span 은 space 일때 키보드에 아무표시도 하지않고 나머지 span은 키보드에 keyData의 데이터들을 표시하겠다는 조건부 코드이다.
  
  
  
``` javascript
  data () {
    return {
      KeyData,
      shiftIndex: 0,
      capsLock: 0,
      lang: 'kr',
      keyArr: [],
      keyValue: null
    }
  },
```
**`KeyData`:** import 한 key data  
**`shiftIndex`:** shift 키의 on/off 값  
**`capsLock`:** capsLock 키의 on/off 값  
**`lang`:** ko <=> en change  
**`keyArr`:** 키보드 클릭 이벤트로 발생한 키 값들의 배열  
**`keyValue`:** keyArr 을 hangulejs 로 변환하여 리턴해줄 값  
  
  
  
**methods**
``` javascript
  classObject (key) {
    switch (key) {
      case 'BackSpace':
        return { delete: true }
      case 'Tab':
        return { tab: true }
      case 'CapsLock':
        return { caps: true }
      case 'Enter':
        return { enter: true }
      case 'Shift':
        if (this.shiftIndex === 1) {
          return { shift: true, active: true }
        } return { shift: true, active: false }
      case '한/영':
        if (this.lang === 'en') {
          return { lang: true, active: true }
        } return { lang: true, active: false }
      case 'space':
        return { space: true }
      default:
        return { none: false }
    }
  },
```
각각의 해당하는 특수키의 class명을 리턴한다.  
각각 키의 넓이 값을 별도로 갖게 되는데에 쓰인다.
  
  
  
``` javascript
  async keyEvent (key) {
    switch (key) {
      case 'Shift':
      case 'CapsLock':
        if (this.shiftIndex === 1) {
          this.shiftIndex = 0
        } else {
          this.shiftIndex = 1
        }
        break
      case '한/영':
        if (this.lang === 'kr') {
          this.lang = 'en'
        } else {
          this.lang = 'kr'
        }
        break
      case 'BackSpace':
        this.delete()
        break
      default:
        await this.keyArr.push(key)
        this.keyValue = await Hangul.assemble(this.keyArr)
        await this.$emit('getKeyValue', this.keyValue)
        break
    }
  },
```
특수키에따른 이벤트를 발생시키거나 `data()` 에 가진 값들에 변화를 준다.
  
  
  
``` javascript
  default:
    await this.keyArr.push(key)
    this.keyValue = await Hangul.assemble(this.keyArr)
    await this.$emit('getKeyValue', this.keyValue)
    break
```
위의코드가 핵심인데 빈 배열에 특수키를 제외한 일반 키값들은 배열로 push 해주고  
```Hangul.assemble(Arr)``` 을 통해서 배열로되어있는 자음 모음의 값을을 한글이되도록 합쳐준다. 
  
  
  
``` javascript
  async delete () {
    await this.keyArr.pop()
    this.keyValue = await Hangul.assemble(this.keyArr)
    await this.$emit('getKeyValue', this.keyValue)
  }
```
BackSpace 버튼을 클릭했을때 배열에서 마지막값만 제거하고 나머지 배열을 ```Hangul.assemble(Arr)```  로 다시 한글로 합쳐주고 값을 부모에게 리턴해준다.
  
  
  
## 생각정리
- Shift 와 CapsLock이 현재 같은 동작을 하게 되어있는데 이 부분이 수정되어야한다.  
- Shift 는 다른키를 입력시 다시 풀려야하고 쌍자음은 Shift 일때만 나와야한다.  
- CapsLock 는 영문의 대소문자를 누를수 있게 하는 키이고 다시 CapsLock을 누르기전까지는 풀리지 않아야한다.
- 추가로 테마를 설정할 수 있도록 할 예정이다.
  
아직 완전한 기능도하고 베타버전으로 npm 에 퍼블리싱한 상태이다. 자잘한것들을 수정하고 다시 올리고 하다보니 버전은 벌써 ```v0.1.9``` 이다. 다운로드수도 그냥 아무 생각도 없었는데 2020/02/04(화) 기준으로 **219번의** 다운로드가 발생하였다.  
  
kiosk를 염두해두고 만들었다보니 기존의 라이브러리들도있고 수요가 이렇게 있을지는 몰랐는데 그냥 테스트한다고 대충 올렸던 버전을 받았던 사람들에게 좀 미안해진다.  
  
애초에 심플하게만 사용하려고 만들려고했던거라 `Shift / CapsLock / Theme` 등의 기능을 추가하고 더이상의 기능추가는 하지않을생각이다. 이슈나 따로 요청이 들어온다면 그건 그때가서 생각해볼일이지만 아마 git 에서 clone 하여 커스텀 하는쪽이 더 빠를지 싶다.
  
처음으로 npm 에 컴포넌트를 publish 해봤는데 이거 은근 재미있는 경험이다. 현재 회사에 입사하고 여러가지 프로젝트를 하면서 몇가지 만들어 두었던게있는데 이것도 코드를 조금 수정해서 npm 에 한번 올려보려고 한다.
  
  
  
## Result
- [vue-virtual-keyboard-ko](https://www.npmjs.com/package/vue-virtual-keyboard-ko)
- [github repogitory](https://github.com/kangyongseok/vue-virtual-keyboard-ko)
  
## Reference
- [hangul.js](https://github.com/e-/Hangul.js/)
- [Vue 컴포넌트를 오픈소스로 npm에 배포하기](https://velog.io/@ashnamuh/Vue-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4%EB%A1%9C-npm%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0-dxjxfg5v7e)
- [Vue 컴포넌트를 npm에 등록하기](https://steemit.com/vue/@stepanowon/vue-npm)


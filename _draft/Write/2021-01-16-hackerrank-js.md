---
title: "해커랭크 - Skills Certified JS"
categories: 
  - write
tags: 
    - hackerrank
    - javascript
    - Certified
    - Basic
    - test
toc: true
toc_sticky: true
comments:  true
---

## 해커랭크 자바스크립트 검증
이번에 코딩마스터즈 라는곳에서 주최하는 코딩대회에 프론트로 참여해보려고 신청을 했다가 해커랭크라는곳을 알게되었다. 프로그래머스같은 곳인것같은데 영문으로만 되어있는거보니 해외용인것으로 보여진다. 원래대로라면 코딩대회에 참여하고있어야했지만 접속자 폭주로 서버 이슈가 발생해 2시부터 진행하는 대회였지만 글을 작성하는 지금 5시에도 접속이 안된다. 저녁엔 약속을 잡아놨기때문에 어쩔수없이 그냥 포기하고 기술검증을하는 문제풀이가 있어서 차라리 이거나 해보기로 했다.
  
자바스크립트를 신청했고 90분간 2문제를 풀면된다 알고리즘 문제는 아니고 자바스크립트의 기술을 사용할 수 있는지에 대한 최소한의 검증으로 보여졌다. 문제 난이도는 Basic 이라그런지 그렇게 높진않았고 영어로 문제가 나오는 부분만 해결한다면 두문제해서 30분~1시간 사이면 충분히 풀고 남을정도의 시간이었던것같다.

### 문제01
![image](https://raw.githubusercontent.com/kangyongseok/kangyongseok.github.io/master/assets/images/hackerrank_test_js_01.png)  
첫번째 문제는 API를 호출하여 원하는 결과값을 리턴해 낼수 있는지에대한 검증으로 보여졌다.  
문자로된 국가코드를 인자로 받은 함수내에서 endpoint 를 호출하고 해당 국가의 풀네임을 리턴시켜주면 되는 문제였다. 해당 endpoint 는 페이지별로 요청할수도있지만 인자로받은 국가코드로도 목록을 불러올 수 있어서 쉽게 불러와서 해당하는 국가의 풀 네임을 리턴할 수 있었다. async 가 함수에 있는걸 보면 await 를 사용하라는것같아서 그렇게 처리했고 axios 를 사용했는데 상단에 해당 라이브러리를 불러와서 사용하였다.

### 문제02
![image](https://raw.githubusercontent.com/kangyongseok/kangyongseok.github.io/master/assets/images/hackerrank_test_js_02.png)  
두번째문제는 자바스크립트의 클래스를 작성하는걸 알고있는지에 대한 문제같았고 클래스내부에 main에서 불러와 사용되고있는 두개의 메소드를 적절하게 코드를 작성해주면 되는 문제였다.  

  
재미삼아 해봤는데 까다로운 검증과정이라기보다는 그냥 최신 자바스크립트 문법에 대해서 사용할줄 아는가에 대한 검증정도로 보여진다. 복잡한 로직이나 알고리즘이 필요한것은아닌것같다.
  

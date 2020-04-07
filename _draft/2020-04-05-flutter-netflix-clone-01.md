---
title: "인프런 - Flutter 넷플릭스 클론코딩 01"
categories: 
  - study
tags: 
    - lecture
    - online
    - inflearn
    - mobile
    - study
    - android
    - ios
    - flutter
    - dart
    - netflix
    - clone coding
toc: true
toc_sticky: true
comments:  true
---

## Flutter Setting
`Flutter` 를 사용하려면 `Android Studio` 와 `Xcode` 가 둘다 설치가 되어있어야하고  
`Android Studio` 같은경우 `plugin` 에서 `flutter sdk` 를 별도 설치를 해야만 한다. 
  
`vscode` 에서 `flutter` 를 사용하기 위해선 `flutter plugin` 을 설치해주고 터미널에서 `flutter doctor`를 실행하면 아래의 결과와 같이 통과된 사항과 부족한 부분을 친절하게 알려준다.

![Flutter Doctor fail](../assets/images/flutter/flutter_img_03.png)  
  
    
안내해준 문제를 해결하고 다시 `flutter doctor` 를 실행하면 아래와같이 모두 녹색 체크가 들어오면 통과된것이고 이제 `flutter` 코드를 작성하고 앱을 실행할 준비가 된것이다.

![Flutter Doctor success](../assets/images/flutter/flutter_img_02.png)  

`vscode` 기준 상단에서 `디버그 => 디버깅시작` 또는 `F5` 를 눌러서 앱을 실행가능하고   
시뮬레이터는 `command + shoft + p` 를 눌러 `flutter: em` 을 입력하면 `flutter: Launch emulator` 를 선택하고 실행하고자하는 에뮬레이터를 선택해 주면 된다.
![Debug simulator](../assets/images/flutter/flutter_img_01.png)


## 참고
[Flutter 공식문서](https://flutter.dev/docs/get-started/install/macos)  
[mac 에 vscode 로 Flutter 설치하기](https://skuld2000.tistory.com/83)
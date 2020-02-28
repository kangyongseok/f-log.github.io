---
title: "GitFlow 전략"
categories: 
  - gitflow
tags: 
    - git
    - gitflow
    - 협업
    - 프론트개발
    - github
toc: true
toc_sticky: true
comments:  true
---

gitflow 사용하여 팀원간에 협업이 원활하고 코드의 충돌과 잘못된 코드의 배포를 방지하기 위한 규칙 및 전략  
  
## V1
  - [X] develop 브랜치 기준으로 주 단위로 `[ feature/ts_브랜치생성날짜 ]` 로 기준브랜치를 생성  
    ``` javascript
    $ git flow feature start ts_200101
    ```
- [X] 기능추가에 대한 작업은 항상 develop 브랜치 기준으로 feature 브랜치 생성  
    ``` javascript
    $ git flow feature start create_login_component
    ```
- [X] 작업이 완료되면 항상 develop 브랜치와의 rebase -i 작업을통해 최신상태 유지 후 기준브랜치 `[ feature/ts_브랜치생성날짜 ]` 에 merge 진행  
    ``` javascript
    $ git checkout develop
    $ git pull
    $ git checkout create_login_component
    $ git rebase -i develop
    $ git push --force

    // github 에서 master 브랜치를 기준브랜치로 변경후 pull request 진행하고 커밋 메세지 작성후 이상없는지 체크후 merge 진행
    ```


  
- [x] 기준브랜치에서 staging 테스트 서버에 업데이트 하기위해 `[ git flow release start 브랜치명 feature/ts_브랜치생성날짜 ]` 로 기준브랜치 를 바라보는 상태로 release 브랜치를 생성하고 push 진행  
    ``` javascript
    $ git flow release start staging_test feature/ts_200101
    ```
- [x] staging 업데이트 주기는 특이사항 제외하고 점심전, 퇴근전 하루 두번으로 제한  
- [x] 모든 테스트가 OK 사인이 나고 다음 업데이트 반영이 확실한 feature 브랜치 만 develop 브랜치 에 최종적으로 finish 한다.  
- [x] 해당 Flow 의 안정화를 위해 지속적인 커뮤니케이션이 필요
- [x] 약 한달간 진행을 하며 디테일한 이슈들은 보완해 나갈 예정   
 
작성일 : 2020-02-27 목

-------

## V2

위의 방법대로 진행했을때 한가지 문제점이 발생했다.  
내가 생성한 기능추가를 진행중인 개발브랜치에 다른 개발자의 코드가 섞이면 안되고 코드가 합쳐지게되는건 `feature/ts_200101` 브랜치에서만 수행되어야하는데  
정확히 어떤 문제인지 아직 파악은 못했지만 중간에 내가 개발중인 브랜치에서 pull 을 받아야만 push 가 가능하다고 경고가 나오는데 이떄 pull 을 받게되면  
다른 개발자가 작성했던 코드가 내가 개발 진행중인 로컬브랜치에 내려받아지고 머지가 진행된다.  
즉 내가 개발중인 브랜치는 내 코드만 있어야하는데 다른 팀원의 코드가 짬뽕되기때문에 이대로 develop 에 넣을 수 없는 상황이 되버려서 방식을 바꿔야만 했다.  


- [x] 제출 develop 브랜치 기준으로 주 단위로 `feature/ts_생성날짜` 로 기준브랜치 생성
    ```javascript
    $ git flow feature start ts_200101
    ```

- [x] 제출 기능추가작업은 develop 기준으로 feature 생성
    ``` javascript
    $ git flow feature start create_feat_name
    ```

- [x] 제출 작업이 완료되면 develop 에 rebase 진행하고 기준브랜치에서 작업브랜치를 rebase 진행

    ``` javascript
    // feature/create_feat_name
    $ git add .
    $ git commit -m "feat: 메세지 작성"
    $ git push
    $ git checkout develop

    // develop
    $ git pull
    $ git checkout feature/create_feat_name

    // feature/create_feat_name
    $ git rebase -i develop
    $ git push --force
    $ git checkout feature/ts_200101

    // feature/ts_200101
    $ git pull
    $ git rebase -i feature/create_feat_name
    $ git push --force
    ```

- [x] 제출 기준브랜치에서 테스트 서버에 업로드 하기 위한 작업

    ``` javascript
    // feature/ts_200101
    $ git flow release start create_relese_name feature/ts_200101

    // release/create_relese_name
    $ git push
    ``` 


- [x] 제출 테스트서버에서 확인이 끝나고 다음버전에 포함할 기능이면 develop 에 merge

    ``` javascript
    // feature/create_feat_name
    $ git checkout develop

    // develop
    $ git pull
    $ git checkout feature/create_feat_name

    // feature/create_feat_name
    $ git rebase -i develop
    $ git push --force
    $ git flow feature finish create_feat_name
    ```


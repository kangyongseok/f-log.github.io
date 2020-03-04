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
  

## V2

위의 방법대로 진행했을때 한가지 문제점이 발생했다.  
내가 생성한 기능추가를 진행중인 개발브랜치에 다른 개발자의 코드가 섞이면 안되고 코드가 합쳐지게되는건 `feature/ts_200101` 브랜치에서만 수행되어야하는데  
정확히 어떤 문제인지 아직 파악은 못했지만 중간에 내가 개발중인 브랜치에서 pull 을 받아야만 push 가 가능하다고 경고가 나오는데 이때 pull 을 받게되면  
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


## V3
`V2` 에서도 문제가 발생했다.  
  
`ts_branch` 에서 작업 브랜치로 `rebase` 할 때 마다 내 코드와 다른 팀원의 수정한 코드가 매번 충돌했고 그걸 수정이 들어간 횟수만큼 충돌을 해결해야하는 문제가 발생했다.   
  
즉 충돌이 발생하면 그 충돌코드를 작성한 작성자가 없으면 어떤게 맞는 코드인지 확인할 길이 없고  만약 현재 작업중인 개발자가 5명 혹은 10명 그 이상이라면 충돌을 해결하다가 더한 알수없는 버그가 만들어질수도 있는 상황이었다.  
  
그래서 어떤식으로 프로젝트 테스트와 빌드를 하고있는지 상황과 여기에 맞추기위해 어떤 조건을 생각했고 이 조건에 맞춰서 어떻게 `git flow` 를 사용할 생각을 했는지 재정리 하려고 한다.

### 프로젝트관리
현재 유지보수중인 프로젝트는 `git flow` 를 사용하고있고 일주일단위로 추가 수정사항 혹은 UI 요소들을 수정하여 업데이트를 진행하고있다.  

`QA` 는 별도의 `staging` 서버에 업데이트하여 진행하고있고 점심먹기전과 퇴근전 이렇게 2번 업데이트를 진행하고 피드백을받는다.  

이 피드백에서 검수를 받고 다음 업데이트에 포함될 내용이라면 작업한 브랜치를 `finish` 하여 `develop` 에는 깨끗하고 배포가능한 코드만 `merge` 를 진행하려고 한다.

### 조건
1. `develop` 브랜치에는 QA가 통과된 차주에 업데이트 반영될 기능을 담은 코드만 포함되어야한다.  
2. `feature/*` 브랜치에는 다른 작업자의 코드가 포함되면 안되며 순수하게 `feature/*` 를 생성한 생성자의 코드만 담고 있어야 한다.  
3. 서로 다르게 작성된 `feature/*` 브랜치의 작업들을 합쳐서 `staging` 에 업데이트할 짧은 주기를 가진 `기준브랜치`가 있어야한다.
4. 개별 `feature/*` 브랜치는 다른 브랜치의 작업과 의존성이 없는 작업이어야 한다.
5. 만일 서로 다른 개발자가 같은 기능에 대한 의존성을 가진 작업을 진행해야 한다면 별도의 브랜치를 생성하지않고 하나의 브랜치에서 같이 작업해야한다.


### Git Flow
**기준 브랜치 생성**
``` javascript
// ts = test statging
$ git flow feature start ts_200101
```
  
  
**작업 브랜치 생성**
``` javascript
$ git flow feature start feat_01
```
  
  
**작업 브랜치 기준브랜치에 병합 준비**
``` javascript
// feature/feat_01
$ git checkout develop

// develop
$ git pull
$ git checkout feat_01

// feature/feat_01
$ git rebase -i develop
$ git push -force
$ git checkout ts_020101

// feature/ts_200101
$ git pull
```
  
    

**기준브랜치에 병합**
``` javascript
$ git merge feat_01
$ git push
```
  
  
**staging 에 업데이트 작업**
``` javascript
// git flow release start 릴리즈브랜치명 릴리즈를딸브랜치명
$ git flow release start staging_update ts_200101

// release/staging_update
$ git push
```
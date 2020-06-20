---
title: "pyenv 로 python 버전관리"
categories: 
  -  python
tags: 
    - develop
    - python
    - pyenv
    - backend
    - study
toc: true
toc_sticky: true
comments:  true
---

## pyenv 로 할 수 있는 일
- 프로젝트별로 파이썬 버전설정 가능
- 한번에 여러 버전의 파이썬 사용가능

## 버전확인
```console
$ pyenv versions
$ python --version
```

## macOS 설치
```console
$ brew install pyenv
```

## 쉘 설정
쉘의 종류에 맞춰서 마지막 명령어만 바꿔주면 된다.  
.zshrc, .bash_profile, .fishrc 등
```console
$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

## pyenv 사용해서 python 설치하기
```bash
$ pyenv install 3.7.5
$ pyenv versions
* system (set by /Users/flynn/.pyenv/version)   
  3.7.5
```

## 설치한 버전으로 변경하기 (global)
```console
$ pyenv global 3.7.5
$ python --version
Python 3.7.5
```

## 프로젝트별로 설정하기
```console
$ pyenv local 3.7.5
```

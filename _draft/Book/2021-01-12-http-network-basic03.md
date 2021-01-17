---
title: "그림으로 배우는 Http & Network Basic"
categories: 
  -  book
tags: 
    - book
    - Http
    - network
    - 내용정리
    - 자기계발
toc: true
toc_sticky: true
comments:  true
---

## HTTP와 연계하는 웹서버
HTTP/1.1 에서는 하나의 HTTP 서버에 여러개의 웹 사이트를 실행하는것이 가능하다. 이를 위해서 가상 호스트라는 기능을 사용한다. 
  
인터넷에서 도메인명은 DNS 에 의해서 IP 주소로 변환하고 엑세스하기때문에 리퀘스트가 서버에 도착한 시점에서 서버안에 서로 다른 도메인이 있을경우 어느쪽에 대한 접근인지 알기가 어렵다. 따라서 호스트명와 도메인명을 완전하게 포함하여 URI를 지정하거나 Host 헤더필드에서 지정해야만 한다.

## 통신중계 : 프록시, 게이트웨이, 터널

### 프록시
서버와 클라이언트의 중계프로그램으로 리퀘스트와 리스폰스를 각각 서버와 클라이언트로 전송해주는 역할을 한다. 클라이언트로부터 받은 리퀘스트를 리소스를 가지고있는 오리진 서버로 전달하는 역할을 기본적으로 하고 리스폰스또한 프록시 서버를 통해서 클라이언트로 되돌아 온다.
  
필요에 따라서는 프록시를 여러번 경유할 수 있는데 Via 헤더필드에 어떤 프록시를 경우하는지 지정해야한다. 프록시 서버를 사용하는 이유는 엑세스제한이나, 엑세스 로그를 획득하는 정책을 지키려는 목적으로 사용하기도 한다.

### 게이트웨이
다른 서버를 중계하는 서버로 클라이언트로부터 수신한 요청에 대해서 리소르를 보유한 서버인척을 한다. 경우에 따라서는 클라이언트에서는 게이트웨이인지 알지 못하는 경우도 있다. 프록시와 유사한 통신을 하지만 HTTP 서버 이외의 서비스를 제공하는 서버와 통신을 한다는 점에서 다르다. 
  
데이터베이스에 접속해 SQL 쿼리로 데이터를 얻거나, 쇼핑사이트에서 결제시스템과 연계될때 사용되기도한다.

### 터널
서로 떨어진 두대의 클라이언트와 서버사이응 중계하며 접속을 주선하는 프로그램이다. 


## 캐시
캐시는 프록시 서버와 클라이언트 로컬 디스크에 보관된 리소스 사본을 가르킨다. 이러한 캐시를 사용하면 리소스를 가진 서버에의 엑세스를 줄여 통신량과 시간을 절약하는것이 가능하다. 즉 요청이 있을때마다 오리진서버로 요청을하고 다시 리소스를 내려받는것이 아닌 캐시에 이미 등록된 리소스가 있다면 오리진서버까지 가지않고 캐시에서 처리를하여 응답을 보내줄 수 있다.
  
이런 캐시에는 유효기간이 존재한다. 오리진서버에는 결국 리소스가 갱신되는 경우가 있는데 이때 캐시는 갱신되기전의 오래된 내용을 리소스로 보내줄 수도있다. 그래서 때에따라 다시 오리진서버에서 새로운 리소스를 획득하러 가는 경우가 있다.
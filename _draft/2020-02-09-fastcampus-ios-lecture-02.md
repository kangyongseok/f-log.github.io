---
title: "FastCampus IOS (02)"
categories: 
  - study
tags: 
    - lecture
    - online
    - fastcampus
    - IOS
    - Swift
    - networking
    - study
toc: true
toc_sticky: true
comments:  true
---

IOS HTTP

HTTP: 서버와 클라이언트간에 데이터를 주고받는 방식

동작방식
클라이언트는 서버에게 Request Message 를 보냄
서버는 요청에 따른 Response Message 를 전달해줌

HTTP Request Method
- POST
- GET
- UPDATE
- DELETE

Content-Type
text/html
application/json
image/png
video/mpeg

Mac Store
Rested and Postman: http 테스트하는 툴
itunes search api: 무료로 사용 가능한 api

URLSession Api
URLSessionConfiguration
URLSessionTask
- URLSessionDataTask
- URLSessionUploadTask
- URLSessionDownloadTask


``` swift

let urlstring = "https://itunes.apple.com/search?term=jack+johnson&entity=musicVideo"
let url = URL(string: urlstring)
url?.absoluteString
url?.scheme
url?.host
url?.baseURL


let baseURL = URL(string: "https://itunes.apple.com")
let relativeURL = URL(string: "search?term=jack+johnson&entity=musicVideo", relativeTo: baseURL)

relativeURL?.absoluteString
relativeURL?.scheme
relativeURL?.host
relativeURL?.path
relativeURL?.query
relativeURL?.baseURL

// URL Components
var urlComponents = URLComponents(string: "https://itunes.apple.com/search?term=jack+johnson&entity=musicVideo")
var queryItem = URLQueryItem(name: "term", value: "지드래곤")
urlComponents?.queryItems?.append(queryItem)
urlComponents?.url
urlComponents?.string
urlComponents?.queryItems


```
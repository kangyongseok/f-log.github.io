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
<!-- 실습코드 1 -->

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

``` swift
<!-- 실습코드2 -->

let config = URLSessionConfiguration.default
let session = URLSession(configuration: config)

var urlComponents = URLComponents(string: "https://itunes.apple.com/search?media=music&entity=song")!
var queryItem = URLQueryItem(name: "term", value: "지드래곤")
urlComponents.queryItems?.append(queryItem)
let requestURL = urlComponents.url!


let dataTask = session.dataTask(with: requestURL) { (data, response, error) in
//    Client-side Error
    guard error == nil else { return }
    
//    Server-side Error
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode else { return }
    let successRange = 200..<300
    guard successRange.contains(statusCode) else {
//       serverside error handle
        return
    }
    
    guard let resultData = data else { return }
    print("---> Result Data: \(resultData)")
}

dataTask.resume()
```


``` swift
<!-- 실습코드3 실제 요청작업 수행 -->

import UIKit


let config = URLSessionConfiguration.default
let session = URLSession(configuration: config)

var urlComponents = URLComponents(string: "https://itunes.apple.com/search?media=music&entity=song")!
var queryItem = URLQueryItem(name: "term", value: "지드래곤")
urlComponents.queryItems?.append(queryItem)
let requestURL = urlComponents.url!


let dataTask = session.dataTask(with: requestURL) { (data, response, error) in
//    Client-side Error
    guard error == nil else { return }
    
//    Server-side Error
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode else { return }
    let successRange = 200..<300
    guard successRange.contains(statusCode) else {
//       serverside error handle
        return
    }
    
    guard let resultData = data else { return }
    
//    Dara > Object
    
    do {
        let jsonObject = try JSONSerialization.jsonObject(with: resultData, options: [])
        if let dictionary = jsonObject as? [String: Any], let tracks = dictionary["results"] as? [[String: Any]] {
            tracks.forEach({ (track: [String: Any]) in
                let title = track["trackName"]
                let artistName = track["artistName"]
                let thumbnail = track["artworkUrl30"]
                print("---> title: \(title), artist: \(artistName), thumb: \(thumbnail)")
            })
        }
    } catch let error {
        print("---> Result Data: \(error.localizedDescription)")
    }
    
    print("---> Result Data: \(resultData)")
}

dataTask.resume()

```


``` swift
<!-- 실습코드4 리팩토링 -->

import UIKit


let config = URLSessionConfiguration.default
let session = URLSession(configuration: config)

var urlComponents = URLComponents(string: "https://itunes.apple.com/search?media=music&entity=song")!
var queryItem = URLQueryItem(name: "term", value: "지드래곤")
urlComponents.queryItems?.append(queryItem)
let requestURL = urlComponents.url!


// model
struct Track {
    let title: String
    let artistName: String
    let thumbnail: String
}


func parse(data: Data) -> [Track]? {
    
    do {
        let jsonObject = try JSONSerialization.jsonObject(with: data, options: [])
        var parsedTrackList: [Track] = []
        
        if let dictionary = jsonObject as? [String: Any], let tracks = dictionary["results"] as? [[String: Any]] {
            
            tracks.forEach({ (track: [String: Any]) in
                if let title = track["trackName"] as? String,
                    let artistName = track["artistName"] as? String,
                    let thumbnail = track["artworkUrl30"] as? String {
                    
                    let track = Track(title: title, artistName: artistName, thumbnail: thumbnail)
                    parsedTrackList.append(track)
                }
            })
        }
        
        return parsedTrackList
        
    } catch let error {
        print("---> Result Data: \(error.localizedDescription)")
        return nil
    }
}

var trackList: [Track] = []


let dataTask = session.dataTask(with: requestURL) { (data, response, error) in
//    Client-side Error
    guard error == nil else { return }
    
//    Server-side Error
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode else { return }
    let successRange = 200..<300
    guard successRange.contains(statusCode) else {
//       serverside error handle
        return
    }
    
    guard let resultData = data else { return }
    
//    Dara > Object
    
    trackList = parse(data: resultData) ?? []
    print("---> total tracks count: \(trackList.count)")
}

dataTask.resume()


```



``` swift
<!-- 실습코드5 Codable 사용 -->

import UIKit


let config = URLSessionConfiguration.default
let session = URLSession(configuration: config)

var urlComponents = URLComponents(string: "https://itunes.apple.com/search?media=music&entity=song")!
var queryItem = URLQueryItem(name: "term", value: "지드래곤")
urlComponents.queryItems?.append(queryItem)
let requestURL = urlComponents.url!


// model

// Codable 을 사용할 때는 서버에서 보내주는 JSON 형태의 구조를 그대로 따라줘야 한다.
struct Response: Codable {
    let resultCount: Int
    let results: [Track]
}

struct Track: Codable {
    let title: String
    let artistName: String
    let thumbnail: String
    
//    JSON 사용시 decoding 키를 사용자 지정
    enum CodingKeys: String, CodingKey {
        case title = "trackName"
        case artistName = "artistName"
        case thumbnail = "artworkUrl30"
    }
    
//    init?(json: [String: Any]) {
//        guard let title = json["trackName"] as? String,
//            let artistName = json["artistName"] as? String,
//            let thumbnail = json["artworkUrl30"] as? String else {
//            return nil
//        }
//
//        self.title = title
//        self.artistName = artistName
//        self.thumbnail = thumbnail
//    }
}


func parse(data: Data) -> [Track]? {
    
    do {
        let decoder = JSONDecoder()
        let response = try decoder.decode(Response.self, from: data)
        let trackList = response.results
        return trackList
    } catch let error {
        print("---> error: \(error.localizedDescription)")
        return nil
    }
    
//    do {
//        let jsonObject = try JSONSerialization.jsonObject(with: data, options: [])
//        var parsedTrackList: [Track] = []
//
//        if let dictionary = jsonObject as? [String: Any], let tracks = dictionary["results"] as? [[String: Any]] {
//
//            parsedTrackList = tracks.compactMap { json in
//                return Track(json: json)
//            }
//        }
//
//        return parsedTrackList
//
//    } catch let error {
//        print("---> Result Data: \(error.localizedDescription)")
//        return nil
//    }
}

var trackList: [Track] = []


let dataTask = session.dataTask(with: requestURL) { (data, response, error) in
//    Client-side Error
    guard error == nil else { return }
    
//    Server-side Error
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode else { return }
    let successRange = 200..<300
    guard successRange.contains(statusCode) else {
//       serverside error handle
        return
    }
    
    guard let resultData = data else { return }
    
//    Dara > Object
    
    trackList = parse(data: resultData) ?? []
    print("---> total tracks count: \(trackList.count)")
}

dataTask.resume()

```
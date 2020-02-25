---
title: "FastCampus IOS (05) - Firebase"
categories: 
  - study
tags: 
    - lecture
    - online
    - fastcampus
    - IOS
    - Swift
    - firebase
    - study
toc: true
toc_sticky: true
comments:  true
---

## 데이터베이스에 저장한 값 불러오기

**Int**
``` swift
rootRef.child("int").observeSingleEvent(of: .value) {
    snapshot in print("---> \(snapshot)")
    
    let value = snapshot.value as? Int ?? 0
    DispatchQueue.main.async {
        self.firstDataLabel.text = "\(value)"
    }
}
```

**Double**
``` swift
rootRef.child("double").observeSingleEvent(of: .value) {
    snapshot in print("---> \(snapshot)")
    
    let value = snapshot.value as? Double ?? 0.0
    DispatchQueue.main.async {
        self.firstDataLabel.text = "\(value)"
    }
}
```

**observeSingleEvent**  
데이터 베이스의 정보가 변해도 view 에서 보여주는 데이터는 변하지 않는다.  
한번만 호출한다.


**observe**  
데이터 베이스의 정보가 변경될때마다 실시간으로 변한다. 즉 콜백을 계속 호출한다.


## 복잡한 데이터 불러오기

**fetchCustomers() 함수 생성**  
``` swift
 override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
//        updateLabel()
//        saveBasicType()
//        saveCustomers()
    
    fetchCustomers()
}
```
  
**fetchCustomers() 함수정의**  
``` swift
func fetchCustomers() {
    let rootRef = Database.database().reference()
    rootRef.child("customers").observeSingleEvent(of: .value) { (snapshot) in
        
        self.firstDataLabel.text = "Son Dele Kane"
    }
}
```
  
**Customer 초기값 설정 parsing**  
``` swift
struct Customer {
    let id: String
    let name: String
    let books: [Book]
    
    // ----- 여기부터
    init(id: String, name: String, books: [Book]) {
        self.id = id
        self.name = name
        self.books = books
    }
    
    init?(dict: [String: Any]) {
        guard let id = dict["id"] as? String,
            let name = dict["name"] as? String,
            let bookList = dict["books"] as? [[String: Any]] else {
                return nil
        }
        self.id = id
        self.name = name
        self.books = bookList.compactMap{ (dict) -> Book? in
            return Book(dict: dict) // compactMap nil 로 표시되는건 다 버린다.
        }
    }
    //---- 여기까지 추가 
    
    var toDictionary: [String: Any] {
        let booksArray = books.map { $0.toDictionary }
        let dict: [String: Any] = ["id": id, "name": name, "books": booksArray]
        return dict
    }
}
```
  
**Book 초기값 설정 parsing**  
``` swift
struct Book {
    let title: String
    let author: String
    
    // --- 여기부터
    init(title: String, author: String) {
        self.title = title
        self.author = author
    }
    
    init?(dict: [String: Any]) {
        guard let title = dict["title"] as? String,
            let author = dict["author"] as? String else {
                return nil
        }
        self.title = title
        self.author = author
    }
    // --- 여기까지 추가
    
    var toDictionary: [String: Any] {
        let dict: [String: Any] = ["title": title, "author": author]
        return dict
    }
}
```
  
**fetchCustomers() name 출력 위한 함수 작성**  
``` swift
 func fetchCustomers() {
    let rootRef = Database.database().reference()
    rootRef.child("customers").observeSingleEvent(of: .value) { (snapshot) in
        if let results = snapshot.value as? [[String: Any]] {
            let customers = results.compactMap{ Customer(dict: $0) }
//                let names = customers.map{ (customer) -> String in
//                    return customer.name
//                }
            let names = customers.map{ $0.name }
            self.firstDataLabel.text = names.joined(separator: " ")
        }
    }
}
```


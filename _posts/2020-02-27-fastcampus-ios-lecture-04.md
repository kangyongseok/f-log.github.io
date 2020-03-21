---
title: "FastCampus IOS (04) - Firebase"
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

## Firebase RealDataBase 데이터 저장하기

**Basic Type Data Set**
``` swift
let rootRef = Database.database().reference()

rootRef.child("int").setValue(3)
rootRef.child("double").setValue(3.4)
rootRef.child("str").setValue("string value - hello")
rootRef.child("array").setValue(["a", "b", "c"])
rootRef.child("dict").setValue(["id": "someId", "age": 43, "city": "Seoul"])
```
![image](https://raw.githubusercontent.com/kangyongseok/kangyongseok.github.io/master/assets/images/firebase_database_01.png)

**복잡한 형태의 데이터 저장**

``` swift
 override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
    updateLabel()
//        saveBasicType()
    saveCustomers()
    
}

func updateLabel() {
    let rootRef = Database.database().reference()
    rootRef.child("test").observeSingleEvent(of: .value) {
        snapshot in print("---> \(snapshot)")
        
        let firstData = snapshot.value as? String ?? "nothing"
        DispatchQueue.main.async {
            self.firstDataLabel.text = firstData
        }
    }
}
    
func saveBasicType() {
    let rootRef = Database.database().reference()
    
    //        firebase data saving
    //        available data type
    //        - number
    //        - string
    //        - dictionary
    //        - array
    
    rootRef.child("int").setValue(3)
    rootRef.child("double").setValue(3.4)
    rootRef.child("str").setValue("string value - hello")
    rootRef.child("array").setValue(["a", "b", "c"])
    rootRef.child("dict").setValue(["id": "someId", "age": 43, "city": "Seoul"])
}
    
 var id = 0
func saveCustomers() {
    let books = [Book(title: "연가시", author: "Awsome"), Book(title: "책 제목", author: "some thing")]
    let newCustomer1 = Customer(id: "\(id)", name: "son", books: books)
    id += 1
    let newCustomer2 = Customer(id: "\(id)", name: "del", books: books)
    id += 1
    let newCustomer3 = Customer(id: "\(id)", name: "kang", books: books)
    id += 1
    
    let rootRef = Database.database().reference()
    rootRef.child("customers").child(newCustomer1.id).setValue(newCustomer1.toDictionary)
    rootRef.child("customers").child(newCustomer2.id).setValue(newCustomer2.toDictionary)
    rootRef.child("customers").child(newCustomer3.id).setValue(newCustomer3.toDictionary)
}


struct Customer {
    let id: String
    let name: String
    let books: [Book]
    
    var toDictionary: [String: Any] {
        let booksArray = books.map { $0.toDictionary }
        let dict: [String: Any] = ["id": id, "name": name, "books": booksArray]
        return dict
    }
}

struct Book {
    let title: String
    let author: String
    
    var toDictionary: [String: Any] {
        let dict: [String: Any] = ["title": title, "author": author]
        return dict
    }
```

![image](https://raw.githubusercontent.com/kangyongseok/kangyongseok.github.io/master/assets/images/firebase_database_03.png)
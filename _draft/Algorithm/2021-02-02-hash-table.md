---
title: "친구알고리즘"
categories: 
  -  Algorithm
tags: 
    - Algorithm
    - coding
    - javascript
    - 프로그래머스
    - 알고리즘풀이
    - 연습
toc: true
toc_sticky: true
comments:  true
---

알고리즘 문제는 아니고 다른회사에서 일하는 친구가 똥을하나 뒤집어 썼는데 (백엔드 노드를 다룸) 원래 파이썬만 하던친구라... 아무튼 어떤 객체 데이터를 가공해야하는 일이있어 되겠냐고 부탁이 들어와서 진행하게된 알고리즘 풀이이다.


## Input
```javascript
const rows = [
    {
        id: 1,
        name: '남성',
        parent: '0',
    },
    {
        id: 2,
        name: '여성',
        parent: '0',
    },
    {
        id: 3,
        name: '아우터',
        parent: '1',
    },
    {
        id: 4,
        name: '바지',
        parent: '1',
    },
    {
        id: 5,
        name: '상의',
        parent: '1',
    },
    {
        id: 6,
        name: '아우터',
        parent: '2',
    },
    {
        id: 7,
        name: '반팔티',
        parent: '5',
    }
]
```

위와같은 데이터를

## output
```javascript
const data = {
    id: 1,
    name: '남성',
    parent: '0',
    children: [
        {
            id: 3,
            name: '아우터',
            parent: '1',
        },
        {
            id: 4,
            name: '바지',
            parent: '1',
        },
        {
            id: 5,
            name: '상의',
            parent: '1',
        },
    ],
},
.
.
.
```

이런식의 데이터로 가공을 해야하는 일이었다.
  
id가 parent 와 매칭되어서 children 배열에 넣어줘야하고 넣어준 데이터는 제거된 상태로 리턴이 되어야한다.
  
나중에 찾아보니 이런걸 자료구조중에 해시테이블이라고 하는것같았다.

```javascript
const result = rows.filter(category => {
    category.children = []
    rows.map((item, i) => {
        if (category.id === Number(item.parent)) {
            category.children.push(item)
        }
    })
    return category.children.length !== 0 && category.parent === '0'
})

console.log(result)
```

위와같이 코드를 작성하여 데이터를 가공하였다.  
각 카테고리별로 children 으로 빈 배열을 넣어주고 id 와 parent 를 매칭하여 해당 데이터의 children 에 push 를 해주었다. 그리고 children이 있는 품목과 결국엔 큰 카테고리는 parent 가 0으로 들어가는것같아 그것만 리턴하도록 해주었다.
  
쓰다보니 생각난건데 `category.children.length !== 0 ` 이거는 빠져도 될것같다.

## 최종코드
```javascript
const result = rows.filter(category => {
    category.children = []
    rows.map((item, i) => {
        if (category.id === Number(item.parent)) {
            category.children.push(item)
        }
    })
    return category.parent === '0'
})

console.log(result)
```
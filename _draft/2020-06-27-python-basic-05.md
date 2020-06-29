---
title: "Python Basic 05"
categories: 
  -  python
tags: 
    - develop
    - python
    - study
    - database
    - SQLite
    - SQL
    - 외래키
    - foreign 
    - join
toc: true
toc_sticky: true
comments:  true
---

## Album Table
```sql
CREATE TABLE "Album" (
  "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
  artist_id INTEGER,
  "title" TEXT
)
```
위의 테이블을 해석하면 
- id 는 정수에 null 이 절대로 올수 없고 주키에 자동증가하는 테이블에서 유일무이한 값
- artist_id 는 외래키로 사용가능하고 정수
- title 은 text

## Track Table
```sql
CREATE TABLE "Track" (
  "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
  title TEXT,
  album_id INTEGER,
  genre_id INTEGER,
  len INTEGER, rating, INTEGER, count INTEGER
)
```
위 테이블은 Track 테이블로 중복되지 않았었던 값들을 갖고 있고 중복되는 항목에 대해서는 외래키로 받아서 사용할 수 있게끔 테이블이 짜여져 있다.
- id 는 정수에 null 이 올수없으며 주키역할을 하고 데이터가 추가될때마다 자동으로 id 값을 업데이트하는 이 테이블의 유일무이한 값이다.
- 나머지 항목은 첫번째 테이블과 동일한 해석 내용을 담을 수 있다.


## Artist Table
```sql
CREATE TABLE "Artist" (
  "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
  "name" TEXT
)
```

## Genre Table
```sql
CREATE TABLE "Genre" (
  "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
  "name" TEXT
)
```

## INSERT DATA
```sql
INSERT INTO Artist (name) VALUES ('Led Zepplin');
INSERT INTO Artist (name) VALUES ('AC/DC');

INSERT INTO Genre (name) VALUES ('ROCK');
INSERT INTO Genre (name) VALUES ('Metal');

INSERT INTO Album (title, artist_id) VALUES ('Who Made Who', 2);
INSERT INTO Album (tltie, artist_id) VALUES ('IV', 1);

INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('Black Dog', 5, 297, 0, 2, 1)
INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('Stairway', 5, 482, 0, 2, 1)
INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('About to Rock', 5, 313, 0, 1, 2)
INSERT INTO Track (title, rating, len, count, album_id, genre_id) VALUES ('Who Made Who', 5, 207, 0, 1, 2)
```

## Join
만약 해당 테이블들에서 앨범의 제목과 가수의 이름을 동시네 매칭시켜서 보여주길 원한다면 join 을 사용하여 만들어 낼 수 있다.
```sql
SELECT Album.title, Artist.name from Album join Artist on Album.artist_id=Artist.id
```
위의 구문은 앨범에서 타이틀 아티스트테이블에서 이름을 가져올건데 앨범과 아티스트 테이블을 조인한것에서 가지고 올것이다. 그리고 가져올때 조건은 on 이후에 나와있다.   
앨범의 아티스트아이디와 아티스트테이블의 아이디가 같을때만 이라는 조건이 붙어있다.
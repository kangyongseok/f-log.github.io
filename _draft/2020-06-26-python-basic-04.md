---
title: "Python Basic 04"
categories: 
  -  python
tags: 
    - develop
    - python
    - study
    - database
    - SQLite
    - SQL
toc: true
toc_sticky: true
comments:  true
---

## SQL문 정리
### 데이터입력
```sql
-- Users 테이블에 name 에는 kang, email에는 kang198817@naver.com 의 데이터를 추가
INSERT INTO Users (name, email) VALUES ('kang', 'kang198817@naver.com');
```

### 데이터 삭제
```sql
-- Users 테이블에서 email 이 kang198817@naver.com 인 데이터를 찾아서 제거
DELETE FROM Users WHERE email='kang198817@naver.com'
```

### 데이터 갱신
```sql
-- Users 테이블에서 email 이 kang198817@naver.com 을 가진 데이터의 name 값을 YongSuek로 갱신
UPDATE Users SET name='YongSuek' WHERE email='kang198817@naver.com'
```

### 데이터 추출
```sql
-- Users 테이블에서 email 이 kang198817@naver.com 을 찾아서 추출
SELECT * FROM Users WHERE email='kang198817@naver.com'
-- Users 테이블에서 email 기준으로 내림차순으로 정렬
SELECT * FROM Users ORDER BY email DESC
```


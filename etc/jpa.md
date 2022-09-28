---
description: JPA 쿼리 로그 설정
---

# JPA 쿼리 로그 설정하는 법

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-28 Wed**

**last modified : 2022-09-28 Wed**

## \[1] resources/application.properties 설정

### 1) 잘 정렬된 쿼리 보도록 설정하기

```properties
spring.jpa.properties.hibernate.format_sql = true
```

### 2) 물음표 '?' 처리된 바인딩 변수 보기

```properties
logging.level.org.hibernate.type.description.sql = trace 
```

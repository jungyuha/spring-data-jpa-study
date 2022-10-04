---
description: Iterable 형식 변환하기
---

# Iterable 형식 변환하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-10-04 Tue**

**last modified : 2022-10-04 Tue**

## Iterable 다루기

JPA repository에서 queryDSL을 통해  List 형식으로 조회하면 Iterable로 반환을 한다.&#x20;

Iterable 형식을  Dto나 Vo로 변환하여 반환할 일이 많기 때문에 정리하고자 한다.

## \[1] forEach 사용하기 <a href="#foreach" id="foreach"></a>

```java
@Override
public List<String> findAll() {
    Iterable<String> users = Arrays.asList("user1","user2","user3");

    List<String> list = new ArrayList<>();
    users.forEach(list::add);
    return list;
}
```

## \[2] List로 변환하기 <a href="#list" id="list"></a>

```java
@Override
public List<String> findAll() {
    Iterable<String> users = Arrays.asList("user1","user2","user3");

    List<String> list = ImmutableList.copyOf(users);
    return list;
}
```

## \[3] stream으로 변환하기 <a href="#stream" id="stream"></a>

```java
@Override
public List<String> findAll() {
    Iterable<String> users = Arrays.asList("user1","user2","user3");

    List<String> list = StreamSupport.stream(users.spliterator(), false).collect(Collectors.toList());
    return list;
}
```


---
description: JPA Top1 쿼리 구현하기
---

# JPA Top1 쿼리 구현하는 2가지 방법

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-28 Wed**

**last modified : 2022-09-28 Wed**

## 첫번째 방법 : FirstN, TopN 문법 사용하기&#x20;

쿼리 메서드명의 FirstN , TopN 문법을 사용한다.

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findTop1ByUserStatusOrderByCreatedAtDesc(String userStatus);
    User findFirst1ByUserStatusIdOrderByCreatedAtDesc(String userStatus)
}
```

## 두번째 방법 : Pageable 사용하기&#x20;

Pageable을 사용하여 Top 쿼리를 호출 호출 후 List.get(0)을 사용해서 데이터를 가져온다.

이는 JPQL 사용시에도 동일하게 적용된다.

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByUserStatusEqualsAndUserAreaInOrderByCreatedAtDesc(String userStatus,
     List<String> areas, Pageable pageable);
}
```







&#x20;

---
description: 기본 리포지토리 커스터마이징 안 하고 기본 JpaRepository를 상속받은 레포지토리에 QueryDSL을 구현한다.
---

# 스프링 데이터 Common 8. QueryDSL 기본 구현하기

## \[1] 레파지토리에 **QuerydslPredicateExecutor 추가**

일전에 만든 레파지토리에 QuerydslPredicateExecutor를 추가한다.

```java
public interface AccountRepository extends JpaRepository<Account, Long>
, QuerydslPredicateExecutor<Account> {
}

```

QuerydslPredicateExecutor를 추가하면 **QuerydslPredicateExecutor**가 제공하는 **** 다음 2가지 메서드를\
사용할 수 있다.

![](<../.gitbook/assets/image (1).png>)

## \[2] queryDSL 작성하기

테스트 코드에 queryDSL을 작성해본다.

#### 1. 쿼리 생성

#### \[예시] account 엔티티의 firstName이 containsIgnore로 'keesun'를 가짐과 동시에 lastName이 startWith로 'balk'을 가지는 쿼리를 생성해본다.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

#### 2. findOne 메서드로 predicate를 넣어 쿼리 Language를 만든다.&#x20;

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p> 테스트 코드</p></figcaption></figure>













## \[2]&#x20;
---
description: 스프링 데이터 Common이 지원하는 Null 처리 방법
---

# 스프링 데이터 Common 3. Null 처리

**기록 ✍️**

#### author : jung yuha

#### first registered : 2022-09-20 Tue

#### last modified : 2022-09-20 Tue

## \[1] Optional을 이용한 Null처리(자바 8이상)

* 스프링 데이터 2.0 부터 자바 8의 ptional을 지원한다.

#### 리턴값을 Optional로 쓰면 리턴값이 널이어도 널로 체크를 하지 않고 Optional 인터페이스가 제공하는 기능을 사용해서 Null 검사를 할 수 있다.

### 1) 리턴값이 Optional인 경우

<figure><img src="../.gitbook/assets/image (5) (2) (2).png" alt=""><figcaption><p> 단일 리턴값이 Optional인 경우</p></figcaption></figure>

### 2) 리턴값이 Optional이 아닌 경우

#### 단일 리턴값이 반환되는 경우

*   &#x20;Optional이 없는 상태에서 단일값을 조회하는 쿼리에 null값이 반환된다면 결과로는 **그냥 null이 나온다.**

    <figure><img src="../.gitbook/assets/image (11) (2).png" alt=""><figcaption><p> 리턴값이 Optional이 아닌 경우 null값이 반환된다.</p></figcaption></figure>
*   Optional이 없는 상태에서 단일값을 조회하는 쿼리 결과는 **그냥 null이 나오므로 isNull로 테스트**한다.

    <figure><img src="../.gitbook/assets/image (1) (3).png" alt=""><figcaption><p> null이 나오므로 isNull로 테스트</p></figcaption></figure>

#### 리스트 데이터가 반환되는 경우

* Optional이 없는 상태에서 리스트를 조회하는 쿼리 결과는 **비어있는 컬렉션이 반환된다.**\
  즉 ,널이 반환되는 것이 아니므로 널체크 구문도 적용되지 않는다.
  *

      <figure><img src="../.gitbook/assets/image (4) (4) (1).png" alt=""><figcaption><p> <strong>비어있는 컬렉션이 반환</strong></p></figcaption></figure>



### 3) Optional 인터페이스를 통해 Null 검사하기

* Optional 타입은 isEmpty() 메서드로 널값을 체크할 수 있다.

#### Optional 인터페이스가 제공하는 기능

* isPresent() : 값의 여부를 확인한다.
*   orElse() : 값이 있으면 조회한 값을 가져오고 값이 없는 경우 대체 인스턴스를 반환하는 기능이다.

    <figure><img src="../.gitbook/assets/image (2) (1) (3) (1).png" alt=""><figcaption><p> Optional의 orElse()</p></figcaption></figure>
*   orElseThrow() : 값이 있으면 조회한 값을 가져오고 값이 없는 경우 예외를 던지는 기능을 한다.

    <figure><img src="../.gitbook/assets/image (15) (1) (1).png" alt=""><figcaption><p> Optional의 orElseThrow()</p></figcaption></figure>

    *   Optional이 없으면 다음과 같이 구현한다. 하지만 Optional로 처리하는 것을 권장한다!&#x20;

        <figure><img src="../.gitbook/assets/image (7) (2) (1).png" alt=""><figcaption><p> Optional없이 orElseThrow() 구현</p></figcaption></figure>

## \[2] Null 어노테이션 (스프링 프레임워크)

* 스프링 프레임워크 5.0부터 지원하는 Null 애노테이션을 통해 런타임시에 널여부를 확인하는 기능을 사용할 수 있다.

#### Null 어노테이션의 종류 몇 가지를 살펴보자

*   **@NonNull**

    <figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p> 데이터 삽입시 Not Null 제약조건 추가</p></figcaption></figure>
*   **@Nullable**

    <figure><img src="../.gitbook/assets/image (3) (1) (3).png" alt=""><figcaption><p> 반환되는 값이 Null이 될 수 있음을 명시한다.</p></figcaption></figure>


* **@NonNullApi** &#x20;

```java
@org.springframework.lang.NonNullApi
package com.yuha.jpaPractice;
```

* 패키지 레벨에서 붙일 수 있는 어노테이션이다.
* 패키지 안에 있는 모든 메서드의 파라미터에 **@NoNull**을 붙인 모습과 동일해진다. (하지만 거의 안 쓴다.)

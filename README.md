---
description: 스프링 데이터 Common 4. 쿼리 만들기
---

# 스프링 데이터 Common 4. 쿼리 만들기

## **\[1] 메소드 이름을 분석해서 쿼리 만들기 (CREATE)**

Spring Data Jpa가 메서드 이름을 분석해서 쿼리를 직접 만든다.

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/8e0c182b-32a7-40e9-bbe6-8cd413cb0929/image.png" alt=""><figcaption><p>create 방식 예</p></figcaption></figure>

### 쿼리 만드는 방법

**리턴타입 {접두어}{도입부}By{프로퍼티 표현식}(조건식)\[(And|Or){프로퍼티 표현식}(조건식)]{정렬 조건} (매개변수)**

|          |                                                                     |
| -------- | ------------------------------------------------------------------- |
| 접두어      | **Find, Get, Query, Count, ...**                                    |
| 도입부      | **Distinct, First(N), Top(N)**                                      |
| 프로퍼티 표현식 | **Person.Address.ZipCode => find(Person)ByAddress\_ZipCode(...)**   |
| 조건식      | **IgnoreCase, Between, LessThan, GreaterThan, Like, Contains, ...** |
| 정렬 조건    | **OrderBy{프로퍼티}Asc\|Desc**                                          |
| 리턴 타입    | **E, Optional\<E>, List\<E>, Page\<E>, Slice\<E>, Stream\<E>**      |
| 매개변수     | **Pageable, Sort**                                                  |

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/940e49cf-068c-4d26-bbc0-b4d31584380e/image.png" alt=""><figcaption><p>comment 엔티티 예시</p></figcaption></figure>

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/a5e25271-e059-4880-be06-21d9171d10bb/image.png" alt=""><figcaption><p>create 방식 예시</p></figcaption></figure>

## \[2] 미리 정의해 둔 쿼리 찾아 사용하기(USE\_DECLARED\_QUERY)

### JPQL로 작성하기&#x20;

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/f9521c33-ffc1-4a71-982a-89132b481a2c/image.png" alt=""><figcaption><p>JPQL 사용 예시</p></figcaption></figure>

### Native SQL로 작성하기

Native SQL로 작성할 땐 **nativeQuery = true** 를 추가한다.&#x20;

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/f2e01f7b-499e-40b6-800b-fa55fb518524/image.png" alt=""><figcaption><p>nativeQuery 사용 예시</p></figcaption></figure>

Native SQL로 작성할 땐 **nativeQuery = true** 를 추가한다.&#x20;

#### 사용하는 이유

* 메소드 이름으로 쿼리를 표현하기 힘든 경우에 사용한다.
* 저장소 기술에 따라 다르다.
  * JPA: **@Query** , **@NamedQuery**

1. Seect **Use Case Subject** in **Toolㄹㅇㄹbox**.
2. Drag on the diagram as the size of Use Case Subject.&#x20;

You can use **QuickEdit** for Model Element (See [Model Element](broken-reference)).

## Use Case Subject

To create an Use Case Subject:

1. Select **Use Case Subject** in **Toolbox**.
2. Drag on the diagram as the size of Use Case Subject.

You can use **QuickEdit** for Model Element (See [Model Element](broken-reference)).

## Use Case Subject

To create an Use Case Subject:

1. Select **Use Case Subject** in **Toolbox**.
2. Drag on the diagram as the size of Use Case Subject.

You can use **QuickEdit** for Model Element (See [Model Element](broken-reference)).

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
  * JPA: **@Query** , **@NamedQuery** 사용하는 이유

### **@Query 어노테이션이 먼저 적용되는 원리**

1. **QueryLookupStrategy** 인터페이스에 정의되어있는 **USE\_DECLARED\_QUERY**

![USE\_DECLARED\_QUERY](https://velog.velcdn.com/images/yooha9621/post/29dd7233-5727-4528-81ca-3374849ef0ba/image.png)

2\. **JpaQueryLookupStrategy.java**  에 **fromQueryAnnotation**이라는 이름으로 맨 첫번째 우선순위 설정이 되어있다.

![fromQueryAnnotation](https://velog.velcdn.com/images/yooha9621/post/bd27cb79-3a9e-4a2d-8c8e-579ced287381/image.png)

## \[3] 미리 정의한 쿼리 찾아보고 없으면 만들기 (CREATE\_IF\_NOT\_FOUND)

* 기본값이다.
* 첫 번째 방법과 두 번째 방법을 혼합한 것이다.
* 메서드 명을 통해 먼저 적절한 쿼리를 찾고 없는 경우 미리 정의해 둔 쿼리를 찾아 사용한다.
* **@EnableJpaRepositories**의 **QueryLookupStrategy**의 속성을 통해 설정이 가능하다.

![QueryLookupStrategy](https://velog.velcdn.com/images/yooha9621/post/2890e9c2-9bdc-4037-9a90-1fdd46ae9eab/image.png)

&#x20;3가지 속성을 확인할 수 있다.

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/3dbbbfa4-37ec-43b3-b1aa-5187c7a4463b/image.png" alt=""><figcaption><p><strong>QueryLookupStrategy</strong></p></figcaption></figure>

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

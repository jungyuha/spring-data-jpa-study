---
description: 스프링 데이터 Common 4. 쿼리 만들기
---

# 스프링 데이터 Common 4. 쿼리 만들기

**기록 ✍️**

#### author : jung yuha

#### first registered : 2022-09-20 Tue

#### last modified : 2022-09-21 Wed

## **\[1] 메서드 이름으로 쿼리 메서드 만들기 (CREATE)**

Spring Data Jpa가 메서드 이름을 분석해서 쿼리를 직접 만든다.

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/8e0c182b-32a7-40e9-bbe6-8cd413cb0929/image.png" alt=""><figcaption><p>create 방식 예</p></figcaption></figure>

### 쿼리 만드는 방법

**리턴타입 {접두어}{도입부}By{프로퍼티 표현식}(조건식)\[(And|Or){프로퍼티 표현식}(조건식)]{정렬 조건} (매개변수)**

* 리턴타입으로는 보통 도메인 객체 리스트 또는 도메인 타입(단일) 또는 Optional

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

### JPQL로 작성하기

* 기본값은 JPQL로 작성해야한다.&#x20;

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/f9521c33-ffc1-4a71-982a-89132b481a2c/image.png" alt=""><figcaption><p>JPQL 사용 예시</p></figcaption></figure>

### Native SQL로 작성하기

Native SQL로 작성할 땐 **nativeQuery = true** 를 추가해야 한다.&#x20;

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

* 스프링 데이터 Common의 기본 전략이다.
* 첫 번째 방법과 두 번째 방법을 혼합한 것이다.
* 메서드 명을 통해 먼저 적절한 쿼리를 찾고 없는 경우 미리 정의해 둔 쿼리를 찾아 사용한다.
* **@EnableJpaRepositories**의 **QueryLookupStrategy**의 속성을 통해 설정이 가능하다.

![QueryLookupStrategy](https://velog.velcdn.com/images/yooha9621/post/2890e9c2-9bdc-4037-9a90-1fdd46ae9eab/image.png)

&#x20;3가지 속성을 확인할 수 있다. 기본값은 **CREATE\_IF\_NOT\_FOUND** 이다.

<figure><img src="https://velog.velcdn.com/images/yooha9621/post/3dbbbfa4-37ec-43b3-b1aa-5187c7a4463b/image.png" alt=""><figcaption><p><strong>QueryLookupStrategy</strong></p></figcaption></figure>



### 쿼리 만드는 방법

**리턴타입 {접두어}{도입부}By{프로퍼티 표현식}(조건식)\[(And|Or){프로퍼티 표현식}(조건식)]{정렬 조건} (매개변수)**

#### 리턴타입의 종류

* 도메인 객체 리스트 : List\<Post>
* &#x20;도메인 타입(단일) : Post
* Optional 타입 : Optional\<Post>
  *   Page 타입  : Page \<Post>

      * Page타입을 받고 싶으면 매개변수로 Pageable 파라미터를 줘야한다.

      <figure><img src="../.gitbook/assets/image (7) (3).png" alt=""><figcaption></figcaption></figure>

      * 이 때 반환 객체가 Page가 아니라 List로 설정하면 페이징이 되지 않는다.
      * Pageable 타입에는 Sorting (정렬) 기능도 포함되어 있다.
* Slice 타입 : Slice \<Post>

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

## \[4] 테스트 만들어서 확인해보기

### 1. 쿼리 메서드가 제대로 작성됐는지 확인하기

* 굳이 끝까지 돌려보지 않고도 애플리케이션 테스트를 통해 확인할 수 있다.\
  만약에 잘못된 쿼리 메서드이면 뜨지도 않고 에러가 난다!

## \[5] **메서드 이름으로 쿼리 메서드 만들기** 실습

### 1. 기본 실습

#### **\[ 쿼리 메서드 예시 1]**   특정 키워드를 가진 Comment 데이터 조회

```java
List<Comment> findByCommentContains(String keyword);
```

<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption><p><strong>쿼리 메서드</strong> </p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (17) (1).png" alt=""><figcaption><p><strong>테스트</strong></p></figcaption></figure>

**이 때 리스트에 값이 없는 경우 널이 나오지 않고 Empty로 나온다.**

#### **\[ 쿼리 메서드 예시 2]   대소문자 구분없이** 특정 키워드를 가지며 likeCount가 일정수 이상인 Comment 데이터 조회

```java
List<Comment> findByCommentContainsIgnoreCaseAndLikeCountGreaterThan(String keyword,Integer likeCount);
```

<figure><img src="../.gitbook/assets/image (13) (2) (1).png" alt=""><figcaption><p><strong>쿼리 메서드</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (1) (2).png" alt=""><figcaption><p><strong>테스트</strong></p></figcaption></figure>



#### **\[ 쿼리 메서드 예시 3]   대소문자 구분없이** 특정 키워드를 가지며 likeCount로 내림차순 정렬한  Comment 데이터 조회

```java
List<Comment> findByCommentContainsIgnoreCaseOrderByLikeCountDesc(String keyword);
```

<figure><img src="../.gitbook/assets/image (13) (2) (1).png" alt=""><figcaption><p><strong>쿼리 메서드</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9) (2).png" alt=""><figcaption><p>테스트</p></figcaption></figure>

#### **\[ 쿼리 메서드 예시 4]   대소문자 구분없이** 특정 키워드를 가지며 페이징 처리한  Comment 데이터 조회

```java
Page<Comment> findByCommentContainsIgnoreCase(String keyword , Pageable pageable);
```

**테스트 코드**

<figure><img src="../.gitbook/assets/image (5) (3).png" alt=""><figcaption><p><strong>테스트</strong></p></figcaption></figure>

```java
PageRequest pageRequest = pageRequest.of(0,10,Sort.by(Sort.Direction.DESC,LikeCount));
Page<Comment> findByCommentContainsIgnoreCase("Spring" , pageRequest); 
```

* **PageRequest 타입**으로 반환하는데 PageRequest는 일종의 페이지 타입이다.
* Pagerequest 타입은 static한 세 가지 타입을 지원한다.
  *   **page , size**를 제공하는 타입&#x20;

      <figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
  *   **page , size , sort**를 제공하는 타입



      <figure><img src="../.gitbook/assets/image (7) (2) (2).png" alt=""><figcaption></figcaption></figure>
  *   **page , size , 정렬하는 direction , 정렬하는 properties**를 제공하는 타입

      <figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
* Page 타입의 **getNumberOfElements()** 메서드는 현재 가지고 온 데이터의 갯수를 나타낸다.

#### **\[ 쿼리 메서드 예시 5]   대소문자 구분없이** 특정 키워드를 가지며 Stream 타입으로  Comment 데이터 조회

* Stream 타입은 **try-with-resource문**을 사용해야한다. Stream은 닫아주어야 하기 때문이다.

**테스트 코드**

```java
try (Stream<Comment> comments = commentRepository.findByCommentContainsIgnoreCase(String keyword , Pageable pageable))
{
    Comment firstComment = comments.findFirst().get();
    assertThat(firstComment.getLikeCount()).isEqualTo(100);
}
```

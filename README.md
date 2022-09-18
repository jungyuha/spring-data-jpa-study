---
description: 스프링 데이터 Common이 제공하는 Repository 살펴보기
---

# 스프링 데이터 Common 4. 쿼리 작성해보기

기타&#x20;

* author : jung yuha
* last modified : 2022-09-18 Sun&#x20;

## \[1] 기존 Spring Data Jpa를 통한 레포지토리 작성 방법

*   JpaRepository를 상속받아 레포지토리를 만들었다.

    <figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption><p> JpaRepository를 상속받은 모습</p></figcaption></figure>

### 1) JpaRepository

* JpaRepository는 스프링 데이터 Jpa가 제공하는 인터페이스이다.

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption><p> JpaRepository는 스프링 데이터 Jpa가 제공하는 인터페이스</p></figcaption></figure>



*   JpaRepository는 PagingAndSortingRepository 인터페이스를 상속받는다.&#x20;

    <figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption><p> PagingAndSortingRepository 인터페이스를 상속받은 모습</p></figcaption></figure>

### 2) PagingAndSortingRepository

* **PagingAndSortingRepository** 인터페이스부터가 **스프링 데이터 Common**에 속하는 시작점이다.
* 페이징과 Sorting(정렬)을 지원하는 메서드가 있다.
*   기본적인 CRUD 기능을 가진 **CrudRepository** 인터페이스를 상속받는다.

    <figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption><p><strong>CrudRepository</strong> 인터페이스를 상속받은 모습</p></figcaption></figure>

### 3) **CrudRepository**

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption><p> <strong>CrudRepository</strong></p></figcaption></figure>

* 실질적으로 기능을 구현하고 있지 않는 단순한 마커 인터페이스인 Repository를 상속받는다.
* 가장 기본적인 CRUD 기능들을 지원한다.
  * save :&#x20;
* **@NoRepositoryBean** 어노테이션이 달려있다.
  * 단순한 마커 인터페이스인 Repository를 상속받았기 때문에 spring data jpa의 여러 레포지토리가 실제 빈을 생성하는 것을 방지하기 위함이다.
  * 즉 , 실제 레파지토리가 아님을 표시하기 위한 수단인 것이다.
  *   따라서 중간단계의 레파지토리에는 모두 @NoRepositoryBean 어노테이션이 달려있다.

      <figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p> NoRepositoryBean 어노테이션이 달려있는 JpaRepository</p></figcaption></figure>

      <figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p> @NoRepositoryBean 어노테이션이 달려있는 PagingAndSortingRepository</p></figcaption></figure>

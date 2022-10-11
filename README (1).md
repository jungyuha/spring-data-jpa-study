---
description: 스프링 데이터 Common이 제공하는 Repository 살펴보기
---

# 스프링 데이터 Common 1. 리포지토리

**기록 ✍️**

#### author : jung yuha

#### last modified : 2022-09-18 Sun

## \[1] 기존 Spring Data Jpa 통한 레포지토리 작성 방법

*   JpaRepository를 상속받아 레포지토리를 만들었다.

    <figure><img src=".gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption><p> JpaRepository를 상속받은 모습</p></figcaption></figure>

### 1) JpaRepository

* JpaRepository는 스프링 데이터 Jpa가 제공하는 인터페이스이다.

<figure><img src=".gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption><p> JpaRepository는 스프링 데이터 Jpa가 제공하는 인터페이스</p></figcaption></figure>



*   JpaRepository는 PagingAndSortingRepository 인터페이스를 상속받는다.&#x20;

    <figure><img src=".gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption><p> PagingAndSortingRepository 인터페이스를 상속받은 모습</p></figcaption></figure>

### 2) PagingAndSortingRepository

* **PagingAndSortingRepository** 인터페이스부터가 **스프링 데이터 Common**에 속하는 시작점이다.
* 페이징과 Sorting(정렬)을 지원하는 메서드가 있다.
*   기본적인 CRUD 기능을 가진 **CrudRepository** 인터페이스를 상속받는다.

    <figure><img src=".gitbook/assets/image (2) (2) (1).png" alt=""><figcaption><p><strong>CrudRepository</strong> 인터페이스를 상속받은 모습</p></figcaption></figure>

### 3) **CrudRepository**

<figure><img src=".gitbook/assets/image (5) (1) (1).png" alt=""><figcaption><p> <strong>CrudRepository</strong></p></figcaption></figure>

* 실질적으로 기능을 구현하고 있지 않는 단순한 마커 인터페이스인 Repository를 상속받는다.
* 가장 기본적인 CRUD 기능들을 지원한다.
  * save : 하나의 엔티티를 저장하고 저장되 엔티티를 다시 반환한다.
  * saveAll : 여러개의 엔티티(인스턴스)를 한 번에 저장하고 Iterable 타입으로 저장된 엔티티의 목록을 반환한다.
  * findById : 엔티티를 찾은 뒤 자바8 이상부터 지원하는 Optional 타입을 반환하여 널값대신에 Optional 타입으로 반환한다.
  * existsbyId : 엔티티의 존재 유무를 확인할 수 있다.
  * findAll : 어떤 한 엔티티의 모든 데이터를 다 가지고 온다. 실무에서는 거의 쓰지 않으며 테스트 환경에서 가끔 쓰인다.
  * count : 카운팅 기능을 수행한다.
  * deleteById : 삭제 기능을 수행한다.
  * ...
* **@NoRepositoryBean** 어노테이션이 달려있다.
  * 단순한 마커 인터페이스인 Repository를 상속받았기 때문에 spring data jpa의 여러 레포지토리가 실제 빈을 생성하는 것을 방지하기 위함이다.
  * 즉 , 실제 레파지토리가 아님을 표시하기 위한 수단인 것이다.
  *   따라서 중간단계의 레파지토리에는 모두 @NoRepositoryBean 어노테이션이 달려있다.

      <figure><img src=".gitbook/assets/image (2) (1) (1).png" alt=""><figcaption><p> NoRepositoryBean 어노테이션이 달려있는 JpaRepository</p></figcaption></figure>

      <figure><img src=".gitbook/assets/image (6) (1) (1).png" alt=""><figcaption><p> @NoRepositoryBean 어노테이션이 달려있는 PagingAndSortingRepository</p></figcaption></figure>

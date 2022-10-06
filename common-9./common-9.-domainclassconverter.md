---
description: '스프링 데이터 Common 9. 웹 기능 : DomainClassConverter'
---

# 스프링 데이터 Common 9. 웹 기능 : DomainClassConverter

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-10-04 Tue**

**last modified : 2022-10-04 Tue**

## **이번 단원의 목표**

* **DomainClassConverter**의 간단한 기능을 구현할 수 있다.
* **MockMVC를 통한 테스트 방법**을 알 수 있다.

## \[1] 프로젝트 생성

### 1) 의존성 추가

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p> 간단한 의존성 추가</p></figcaption></figure>

* **Spring JPA 추가** : spring-boot-starter-data-jpa
* **Spring Web 추가** : spring-boot-starter-web
* **h2 database 추가**
* **Spring test 추가**

### 2) Entity 생성 : Post

![](<../.gitbook/assets/image (42).png>)

### 3) Repository 생성  : PostRepository

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### 4) Controller 생성 : PostController

#### 1. Post 조회 핸들러 생성

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

#### 2. 레파지토리 선언&#x20;

* 레파지토리 선언 방법 (1) : **Autowired 를 통한 주입**
  * ![](<../.gitbook/assets/image (15).png>)
*   레파지토리 선언 방법 (2) : **생성자를 통한 주입**

    <figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

## \[2] **MockMvc**를 통해 Post 조회 테스트 작성하기

### 1) 테스트 코드 작성

새로 저장한 Post의 Id를 요청하여 **MockMvc**를 통해 해당 Post를 조회하는 테스트를 작성한다.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### 2) 테스트 결과

* **Insert** 는 테스트 코드에서 실행되고
* **Select**는 애플리케이션 코드에서 실행된다.

## \[3] DomainClassConverter란?

### 1) Converter란 ?

* 어떤 하나의 타입을 다른 타입으로 변환하는 인터페이스이다.
* DomainClassConverter가 기본으로 참조하는 인터페이스이다.

#### Spring Formatter과의 차이점

* 둘 다 타입을 다른 타입으로 변환하는 인터페이스인 건 동일하나 **Spring Formatter의 출발 타입은 무조건 String이다.**

### 2) DomainClassConverter의 주요 컨버터 2가지

#### 1. ToEntityConverter : 어떤 한 엔티티의 Id를 받아 해당 Id를 가진 엔티티로 변환한다.

* 레파지토리를 사용해서 Id를 받아 findById 메서드를 실행한다.
*   때문에 아래 테스트 코드와 일치하게 된다.

    <figure><img src="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FqtYm6Ywqj38cH80t6PRm%2Fuploads%2F6AEtGCToclZdiKaFcTUs%2Fimage.png?alt=media&#x26;token=2d6ed0c3-4dad-4223-abaa-66250d99263a" alt=""><figcaption></figcaption></figure>
* 따라서 아래와 같이 **@PathVariable을 Post로 바꾸고 findById** **메서드를 생략해도** 결과는 똑같다.
  *   **단 , @PathVariable에 "id"를 꼭 명시해주어야한다.** \
      **왜냐하면 더이상 @PathVariable안의 변수명과 Post 타입의 변수명인 "post"가 일치하지 않기 때문이다.**

      <figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption><p> <strong>@PathVariable을 Post로 바꾸고 findById</strong> <strong>메서드를 생략해도</strong> 결과는 똑같다.</p></figcaption></figure>

#### 2. ToIdConverter : 어떤 한 엔티티의 Id 타입으로 변환한다.

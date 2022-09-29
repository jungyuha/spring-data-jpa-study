---
description: 스프링 데이터 Common의 리포지토리 테스트 코드 만들기
---

# 스프링 데이터 Common 1. 리포지토리 테스트 코드 만들기

**기록 ✍️**&#x20;

#### author : jung yuha

#### first registered : 2022-09-16  Sun

#### last modified : 2022-09-17  Mon

## \[1] H2 데이터베이스 의존성 추가

* **InMemory 데이터베이스**로 H2 데이터베이스를 pom.xml에 추가한다.

<figure><img src="../../.gitbook/assets/image (4) (2) (1).png" alt=""><figcaption></figcaption></figure>

## \[2] 테스트 파일 생성

### 1) 테스트 관련 어노테이션 추가

#### @RunWith(SpringBootRunner.class) 어노테이션 추가

* 테스트 파일을 만들 때는 @RunWith(SpringBootRunner.class) 어노테이션을 추가해야한다.

#### @DataJpaTest 어노테이션 추가

* DB 레이어 수준의 테스트 범위만 테스트할 수 있게 만드는 어노테이션으로 스프링 부트에서 제공하는 기능이다.
* 테스트 지원 범위가 줄어들어 기존보다 보다 빠르게 테스트를 진행할 수 있다.
* 다른 빈들은 등록이 되지 않으며 레파지토리와 관련된 빈들만 등록된다.
* 테스트에서는 인메모리데이터베이스를 사용하므로 기존에 설정한 데이터베이스와 독립적으로 테스트가 가능해진다.

<figure><img src="../../.gitbook/assets/image (3) (1) (2).png" alt=""><figcaption><p> 테스트 관련 어노테이션</p></figcaption></figure>

### 2) CRUD 테스트 코드 작성 1 : save

1. 저장할 엔티티(인스턴스)를 생성한다.
2. 생성한 엔티티를 저장한다.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption><p> CRUD 테스트 코드 작성 1 : save</p></figcaption></figure>

#### 테스트 결과 : 발급된 Id는 반환되지만 실제로 Insert는 되지 않는다.

* 스프링에 있는 data test들은  **@Test**가 붙어있으면 내장된 @Transactional을 통해  **기본적으로 롤백을 시켜버린다.**
* 하이버네이트는 Sync가 필요할 때만 DB에 데이터를 반영한다.(= 실제 쿼리를 수행한다.)
* 기본적으로 롤백을 시켜주는 건 Spring 프레임워크의 기능 + 쿼리를 실제로 실행시키지 않아버리는 건 하이버네이트의 기능 \
  즉, 기능이 짬뽕이 되어 나타난 결과인 것 !
* 만약 실제로 Insert 하고싶다면 (트랜잭션을 완료하고 싶다면) 테스트 메서드 위에 **@Rollback(false)** 어노테이션을 붙이면 된다.

### 3) CRUD 테스트 코드 작성 2 : findAll

<figure><img src="../../.gitbook/assets/image (3) (2) (2).png" alt=""><figcaption><p> findAll 테스트 코</p></figcaption></figure>

#### 테스트 결과 : 모든 데이터가 반환된다.

### 4) pagingAndSortingRepository 테스트 코드 작성 1 : findAll (Pageable)

<figure><img src="../../.gitbook/assets/image (2) (1) (2) (1).png" alt=""><figcaption><p> pagingAndSortingRepository의 findAll (Pageable)</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption><p> findAll (Pageable) 테스트 코드</p></figcaption></figure>

* 0 페이지부터 시작해서 10개씩 보여주는 코드이다.
* 리턴 타입이 **리스트가 아닌 페이지로** 나온다. 따라서 페이지에 대한 여러가지 정보를 확인할 수 있다.
  * **getTotalElements()** : 전체의 갯수
  * **getNumber()** : 현재 페이지 넘버
  * **getSize()** : 한 페이지에서 요청한 elements 갯수
  * **getNumberofElements()** : 현재 페이지에 담긴 Elements 갯수

### 5) **메소드 이름을 분석해서 만든 쿼리**  테스트 코드 작성 1

<figure><img src="../../.gitbook/assets/image (1) (2).png" alt=""><figcaption><p> Jpa Repository 인터페이스에서 구현한 커스텀한 메서드</p></figcaption></figure>

* Jpa Repository 인터페이스에서 구현한 커스텀한 메서드도 추가할 수 있다.
* 이 예시는 특정한 키워드를 가지고 있는 Post의 목록을 페이징해서 찾는 메서드이다.

#### 테스트 코드

```java
Page<Post> page = postRepository.findByTitleContains("spring",PageRequest.of(0,10);
```

<figure><img src="../../.gitbook/assets/image (3) (2) (1).png" alt=""><figcaption><p> postRepository.findByTitleContains 테스트 코드 </p></figcaption></figure>

#### 테스트 결과

<figure><img src="../../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption><p> select 쿼리 실행</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (5) (2) (1).png" alt=""><figcaption><p> 바인드 변수값 확인하기</p></figcaption></figure>




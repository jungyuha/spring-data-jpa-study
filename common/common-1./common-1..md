---
description: 스프링 데이터 Common의 리포지토리 테스트 코드 만들기
---

# 스프링 데이터 Common 1. 리포지토리 테스트 코드 만들기

## \[1] H2 데이터베이스 의존성 추가

* **InMemory 데이터베이스**로 H2 데이터베이스를 pom.xml에 추가한다.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## \[2] 테스트 파일 생성

### 1) 테스트 관련 어노테이션 추가

#### @RunWith(SpringBootRunner.class) 어노테이션 추가

* 테스트 파일을 만들 때는 @RunWith(SpringBootRunner.class) 어노테이션을 추가해야한다.

#### @DataJpaTest 어노테이션 추가

* DB 레이어 수준의 테스트 범위만 테스트할 수 있게 만드는 어노테이션으로 스프링 부트에서 제공하는 기능이다.
* 테스트 지원 범위가 줄어들어 기존보다 보다 빠르게 테스트를 진행할 수 있다.
* 다른 빈들은 등록이 되지 않으며 레파지토리와 관련된 빈들만 등록된다.
* 테스트에서는 인메모리데이터베이스를 사용하므로 기존에 설정한 데이터베이스와 독립적으로 테스트가 가능해진다.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p> 테스트 관련 어노테이션</p></figcaption></figure>

### 2) CRUD 테스트 코드 작성 1 : save

1. 저장할 엔티티(인스턴스)를 생성한다.
2. 생성한 엔티티를 저장한다.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p> CRUD 테스트 코드 작성 1 : save</p></figcaption></figure>

#### 테스트 결과 : 발급된 Id는 반환되지만 실제로 Insert는 되지 않는다.

* 스프링에 있는 data test들은  **@Test**가 붙어있으면 내장된 @Transactional을 통해  **기본적으로 롤백을 시켜버린다.**
* 하이버네이트는 Sync가 필요할 때만 DB에 데이터를 반영한다.(= 실제 쿼리를 수행한다.)
* 기본적으로 롤백을 시켜주는 건 Spring 프레임워크의 기능 + 쿼리를 실제로 실행시키지 않아버리는 건 하이버네이트의 기능 \
  즉, 기능이 짬뽕이 되어 나타난 결과인 것 !

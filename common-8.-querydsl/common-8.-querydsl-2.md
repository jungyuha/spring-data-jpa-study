---
description: 기본 리포지토리 커스터마이징을 한 레포지토리에 QueryDSL을 구현하는 방법
---

# 스프링 데이터 Common 8. QueryDSL 응용 구현하기(신버전)

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-29 Thu**

**last modified : 2022-09-29 Thu**

## 이번 단원의 목표

* [**스프링 데이터 Common 6. 기본 리포지토리 커스터마이징하기**](../common/common-6..md) 단원에서 만든 PostRepository에 queryDSL을\
  적용해 보도록 한다.
* **스프링 데이터 Common 8. QueryDSL 응용 구현하기(신버전)**은 커스텀 레파지토리의 **기본 구현체인 SimpleJpaRepository를 그대로 상속받는 버전이다.**

## \[1] QueryDSL 셋팅하기

### 0) 사전작업

#### 1 . 엔티티 생성 : Book 이라는 엔티티를 생성한다.

![](<../.gitbook/assets/image (36) (1).png>)

#### 2. 레파지토리 생성 : JpaRepository를 상속받아 BookRepository를 생성한다.

<figure><img src="../.gitbook/assets/image (8) (4).png" alt=""><figcaption><p> BookRepository</p></figcaption></figure>

#### 3. 테스트 파일 생성

<figure><img src="../.gitbook/assets/image (1) (6).png" alt=""><figcaption></figcaption></figure>

## \[2] 레파지토리에 **QuerydslPredicateExecutor 추가**

BookRepository에 QuerydslPredicateExecutor를 추가한다.

```java
public interface BookRepository extends JpaRepository<Book, Long>
, QuerydslPredicateExecutor<Book> {
}

```

## \[3] 공통(베이스) 레파지토리 생성

JpaRepository의 기능들도 구현할 수 있도록 JpaRepository를 상속받도록 한다.

<figure><img src="../.gitbook/assets/image (16) (2).png" alt=""><figcaption></figcaption></figure>

## \[4] 공통(베이스) 레파지토리의 구현체 생성

<figure><img src="../.gitbook/assets/image (3) (4).png" alt=""><figcaption></figcaption></figure>

* JpaRepository에 관한 기능들은 SimpleJpaRepository에 구현되어 있다.(따로 구현할 필요가 없다.)
* 생성자를 만들어 빈을 주입받고 전달해주어야 한다.

<pre class="language-java"><code class="lang-java"><strong>public interface CustomRepositoryImpl&#x3C;T,ID extends Serializable>
</strong>Extends SimpleJpaRepository&#x3C;T,ID> implements CustomRepository&#x3C;T,ID> {

   private EntityManager entityManager;

    public CustomRepositoryImpl(JpaEntityInformation&#x3C;T, ?> entityInformation, EntityManager entityManager) {
        super(entityInformation, entityManager);
        this.entityManager = entityManager;
    }
    @Override
    public boolean contains(T entity) {
        return entityManager.contains(entity);
    }
}</code></pre>

## \[5] BookRepository에 대한 기본(공통) 레파지토리 적용

**테스트코드에 일전에 만든 Custon 레포지토리의 contains 기능을 사용하려면 Custom 레포지토리를 기본적으로 사용할 수 있도록 설정해야 한다.**&#x20;

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption><p> <strong>여기에 contains 기능도 넣어보자구</strong></p></figcaption></figure>

#### 기존의 BookRepository의 모습이다.

<figure><img src="../.gitbook/assets/image (8) (4).png" alt=""><figcaption><p> 기존의 BookRepository</p></figcaption></figure>

#### JpaRepository대신 CustomRepository를 상속받은 BookRepository의 모습이다.

CustomRepository의 Contains 기능을 참조할 수 있게 된다.

<figure><img src="../.gitbook/assets/image (10) (4).png" alt=""><figcaption><p> CustomRepository를 상속받은 BookRepository</p></figcaption></figure>

#### 하지만 애플리케이션은 아직 CustomRepository의 Contains 구현체를 알지 못한다.

## \[6] 애플리케이션에 CustomRepository구현체 알리기

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

## \[7] 테스트 실행

테스트 코드에 queryDSL을 작성한다.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

#### [common-8.-querydsl-1.md](common-8.-querydsl-1.md "mention") 에서는 다음과 같은 이유로 동작하지 않았어서...

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

## \[3] queryDSL 작성하기

테스트 코드에 queryDSL을 작성한다.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

#### 다음과 같이 SimpleMyRepsitory 대신 **QuerydslPredicateExecutor의 구현체인 QueryDslJpaRepository 인터페이스를 상속받아 해결했었다.**

<figure><img src="../.gitbook/assets/image (7) (5).png" alt=""><figcaption></figcaption></figure>

#### **하지만 버전이 바뀌면서** 공통 구현체 CustomRepositoryImpl에서 SimpleMyRepsitory를 상속받아도 QueryDSL은 잘 동작한다.&#x20;

<figure><img src="../.gitbook/assets/image (33) (2).png" alt=""><figcaption></figcaption></figure>

### SimpleMyRepsitory를 상속받아도 QueryDSL이 잘 동작하는 이유

<figure><img src="../.gitbook/assets/image (4) (6).png" alt=""><figcaption><p> <strong>QuerydslPredicateExecutor</strong>가 제공하는 <strong></strong> 메서드</p></figcaption></figure>

**QuerydslPredicateExecutor의 구현체가 Extension처럼 확장 구현체를 쓰듯이 변경되었기 때문이다.**

**따라서 QuerydslPredicateExecutor의 구현체를 바로 찾아가기 때문에 QuerydslPredicateExecutor 인터페이스에 대한 추가 구현을 커스텀 리파지토리에 따로 하지않아도 된다.**

****

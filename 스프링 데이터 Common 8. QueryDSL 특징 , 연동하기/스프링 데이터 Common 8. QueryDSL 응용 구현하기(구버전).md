---
description: 기본 리포지토리 커스터마이징을 한 레포지토리에 QueryDSL을 구현하는 방법
---

# 스프링 데이터 Common 8. QueryDSL 응용 구현하기(구버전)

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-28 Wed**

**last modified : 2022-09-29 Thu**

## 이번 단원의 목표

* [**스프링 데이터 Common 6. 기본 리포지토리 커스터마이징하기**](../common/common-6..md) 단원에서 만든 PostRepository에 queryDSL을\
  적용해 보도록 한다.
* **스프링 데이터 Common 8. QueryDSL 응용 구현하기(구버전)**은 커스텀 레파지토리의 **기본 구현체인 SimpleJpaRepository을 QuerydslPredicateExecutor의 구현체인 QueryDslJpaRepository 인터페이스로 대신해 상속받는 버전이다.**

## \[1] QueryDSL 셋팅하기

### 1) 의존성 추가

#### 1. pom.xml에 의존성 추가하기

```xml
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-apt</artifactId>
        </dependency>
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-jpa</artifactId>
        </dependency>
```

* queryDSL은 스프링부트가 의존성 관리를 해준다.따라서 pom.xml에 따로 버전을 명시하지 않아도 된다.
  * <img src="../.gitbook/assets/image (30) (1).png" alt="" data-size="original">
    * **query-apt 모듈**은 쿼리를 생성해준다. :  엔티티 모델을 보고 그 모델에 맞는 **쿼리 Language를 만든다.**
      * 이를 위해서는 **플러그인 설정이 필요**하다.&#x20;

#### 2. 플러그인 설정하기

```xml
<plugin>
    <groupId>com.mysema.maven</groupId>
    <artifactId>apt-maven-plugin</artifactId>
    <version>1.1.3</version>
    <executions>
        <execution>
            <goals>
                <goal>process</goal>
            </goals>
            <configuration>
                <outputDirectory>target/generated-sources/java</outputDirectory>
                <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
            </configuration>
        </execution>
    </executions>
</plugin>
```

* 이 플러그인은 버전관리가 되지 않기 때문에 버전을 직접 명시해야한다.
* 자동생성되는 클래스들은 **target/generated-sources/java** 밑에 두도록 설정한다.
* **querydsl.apt.jpa.JPAAnnotationProcessor**를 사용할 것임을 명시한다. ****&#x20;

#### 3. compile 실행

* maven의 lifecycle에서 컴파일을 실행한다.
  * ![](<../.gitbook/assets/image (6) (2).png>)
* **target/generated-sources/java** 밑에 QPost라는 **클래스가 자동생성**된다.\
  **이는 Post 엔티티에 대한 쿼리 Language를 만들어줄 것이다.**
  * ![](<../.gitbook/assets/image (1) (5).png>)

## \[2] 레파지토리에 **QuerydslPredicateExecutor 추가**

일전에 만든 PostRepository에 QuerydslPredicateExecutor를 추가한다.

```java
public interface PostRepository<Post,Long> extends MyRepository<Post, Long>
, QuerydslPredicateExecutor<Post> {
}
```

## \[3] queryDSL 작성하기

테스트 코드에 queryDSL을 작성해본다.

### 1) 테스트 결과 : 에러

<figure><img src="../.gitbook/assets/image (3) (1) (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (33) (1).png" alt=""><figcaption><p> 에러가 난다.</p></figcaption></figure>

* **에러가 나는 이유** :&#x20;
  * **기본 Repository를 커스터마이징 했기 때문이다 = JpaRepository 의 구현체가 없기 때문이다.**
  * **QuerydslPredicateExecutor 인터페이스에 대한 구현체는 기본 JpaRepository가 가지고 있기 때문이다.**
    * ****![](<../.gitbook/assets/image (25).png>)****
  * **하지만 PostRepository에서 쓰고있는 기본 구현체는 SimpleMyRepository이다.**
    *   **SimpleMyRepository에서 QuerydslPredicateExecutor인터페이스의 구현체를 구현하지 않고다.**

        <figure><img src="../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>
    *   **SimpleMyRepository의 기본 구현체인 SimpleJpaRepository가**\
        **QuerydslPredicateExecutor의 구현체를 구현하고 있긴 하다.(QueryDslJpaRepository에서)**

        <figure><img src="../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>
    *   **하지만 main에서 명시적으로 다음과 같이 기본으로 설정해주었기 때문에 SimpleJpaRepository의 구현체보다 앞서 적용이 되어서 QuerydslPredicateExecutor의 구현체가 보이지 않는 것이다.**

        <figure><img src="../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

### 2) 에러 해결하는법

**QuerydslPredicateExecutor의 구현체인 QueryDslJpaRepository 인터페이스를 SimpleMyRepository의 기본 구현체인 SimpleJpaRepository을 대신해 상속받도록 한다.**

```java
public class SimpleMyRepository<T, ID extends Serializable>
 extends QueryDslJpaRepository<T, ID> implements MyRepository<T, ID> {

    private EntityManager entityManager;

    public SimpleMyRepository(JpaEntityInformation<T, ?> entityInformation, EntityManager entityManager) {
        super(entityInformation, entityManager);
        this.entityManager = entityManager;
    }
    @Override
    public boolean contains(T entity) {
        return entityManager.contains(entity);
    }
}
```

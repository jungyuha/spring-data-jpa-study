---
description: 모든 Entity 레파지토리에 추가하거나 오버라이딩하고싶은 경우 공통으로 레파지토리를 구현하는 방법
---

# 스프링 데이터 Common 6.  기본 리포지토리 커스터마이징하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-22 Thu**

**last modified : 2022-09-26 Mon**

## **이번 단원의 목표**

스프링 데이터 Common 5. 커스텀 리포지토리 만들기에서 본 과정은 **1개의 Entity**에 대한 커스텀 구현체였다.&#x20;

**모든 Entity 레파지토리에 추가하거나 오버라이딩하고싶은 경우** Entity마다 적용하는 건 매우 번거로운 일이므로

이런 경우 **공통으로 레파지토리를 구현하는 방법**을 알 수 있다.

## **\[1]** 기본 리포지토리 커스터마이징하기

### 1. JpaRepository를 대체할 새로운 Repository 만들기

기존에는 JpaRepository를 상속받아 사용했다.

<figure><img src="../.gitbook/assets/image (6) (1) (3).png" alt=""><figcaption><p> JpaRepository를 상속받아 사용</p></figcaption></figure>

#### 모든 Entity에 적용될 Repository를 생성한다.이 때 JpaRepository를 상속받아 만든다.

<pre class="language-java"><code class="lang-java"><strong>public interface MyRepository&#x3C;T,ID extends Serializable> Extends JpaRepository&#x3C;T,ID>{}</strong></code></pre>

### **2. 공통 메서드 만들기**

**어떤 Entity가 persistent Context에 존재하는지 체크하는 공통 메서드를 만들어보도록 하자.**

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p> <strong>어떤 Entity가 persistent Context에 존재하는지 체크한다.</strong></p></figcaption></figure>

또 다른 기능을 참고해서 추가하고 싶다면 **JpaRepository**로 들어가서 원하는 메서드를 복사한 뒤&#x20;

<figure><img src="../.gitbook/assets/image (11) (4).png" alt=""><figcaption><p> <strong>JpaRepository</strong>로 들어가서 원하는 메서드를 복사한다. </p></figcaption></figure>

구현체인 **SimpleJpaRepository**에서 복사한 메서드를 오버라이딩하여 직접 커스텀한다.

<figure><img src="../.gitbook/assets/image (1) (1) (2).png" alt=""><figcaption><p> 복사한 메서드를 오버라이딩하여 직접 커스텀한다.</p></figcaption></figure>

### **3. 공통 메서드 구현체 만들기 :** SimpleJpaRepository 구현체와 우리가 정의한 Myepository도 상속받은 인터페이스 생성

이 때는 굳이 접미어(Impl)을 지키지 않아도 된다.

**SimpleJpaRepository 구현체를 상속받은 뒤 우리가 정의한 MyRepository도 상속을 받아야 원하는 공통 기능을 추가할 수 있다.**

<pre class="language-java"><code class="lang-java">@NoRepositoryBean
<strong>public interface SimpleMyRepository&#x3C;T,ID extends Serializable>
</strong>Extends SimpleJpaRepository&#x3C;T,ID> implements MyRepository&#x3C;T,ID> {}</code></pre>

SimpleJpaRepository 구현체는 스프링 데이터 JPA가 제공하는 가장 기본적인 구현체이며 우리가 JPA 레파지토리를 상속받을 때 가져오는 구현체이다.

⭐️ 중간에 사용하는 레파지토리는 **@NoRepositoryBean** 어노테이션을 꼭 붙여줘야한다.

### **4. 공통 메서드 구현체에 생성자 추가하기**

* **생성자를 추가하는 이유 : SimpleJpaRepository**의 부모의 Super를 호출할 때 **entityInformation, entityManager** 2개의 인자를 꼭 줘야 하기 때문에 이 2개의 인자를 받는 생성자를 추가해야 한다.
* **entityManager를 빈으로 선언하지 않는 이유 : 생성자**를 통해서 **** 이 클래스에서 사용할 수 있는 **entityManager**가 전달이 된다. 따라서 **entityManager**는 빈으로 만들어 지지 않기 때문에 빈으로 만들지 않고 `this.entityManager = entityManager;` 문을 통해서 전달받도록 한다.

```java
public class SimpleMyRepository<T, ID extends Serializable> extends SimpleJpaRepository<T, ID> implements MyRepository<T, ID> {

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

이로써 contains 메서드는 MyRepository를 사용하는 모든 저장소에서 사용이 가능하다.

### 5. main에 BaseClass를 명시한다.

우리가 구현하고자 하는 BaseClass를 **@EnableJpaRepositoriers** 어노테이션에 명시해야한다.&#x20;

<figure><img src="../.gitbook/assets/image (13) (3).png" alt=""><figcaption><p> @EnableJpaRepositories</p></figcaption></figure>

```java
@EnableJpaRepositories(repositoryBaseClass = SimpleMyRepository.class)
```

### 6. 공통 메서드를 사용하기위해 MyRepository를 상속받는다.

```java
public interface PostRepository extends MyRepository<Post, Long> {
}
```

## \[2] 커스터마이징 리포지토리 사용하기

### 1. 커스터마이징 리포지토리가 제공되는 과정 요약

* 1\) **MyRepository**가 제공해주는 Contains 메서드가 제공된다.
* 2\) Contains의 구현체는 **SimpleMyRepository**에서 구현되어있다.
* 3\) SimpleMyRepository의 구현체는 **@EnableJpaRepositories**에서 셋팅이 되어있다.

#### 예시 : Contains를 통해 Save하기 전에 PostRepository에 Post의 존재여부를 확인한 뒤 Save를 진행한다.

<figure><img src="../.gitbook/assets/image (16) (2) (2) (1).png" alt=""><figcaption><p> Post의 존재여부를 확인한 뒤 Save를 진행한다.</p></figcaption></figure>

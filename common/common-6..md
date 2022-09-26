---
description: 모든 Entity 레파지토리에 추가하거나 오버라이딩하고싶은 경우 공통으로 레파지토리를 구현하는 방법
---

# 스프링 데이터 Common 6.  기본 리포지토리 커스터마이징하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-22 Thu**

**last modified : 2022-09-22 Thu**

## **이번 단원의 목표**

스프링 데이터 Common 5. 커스텀 리포지토리 만들기에서 본 과정은 **1개의 Entity**에 대한 커스텀 구현체였다.&#x20;

**모든 Entity 레파지토리에 추가하거나 오버라이딩하고싶은 경우** Entity마다 적용하는 건 매우 번거로운 일이므로

이런 경우 **공통으로 레파지토리를 구현하는 방법**을 알 수 있다.

## **\[1]** 기본 리포지토리 커스터마이징하기

### 첫번째 순서, JpaRepository를 대체할 새로운 Repository 만들기

기존에는 JpaRepository를 상속받아 사용했다.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p> JpaRepository를 상속받아 사용</p></figcaption></figure>

#### 모든 Entity에 적용될 Repository를 생성한다.이 때 JpaRepository를 상속받아 만든다.

<pre class="language-java"><code class="lang-java"><strong>public interface MyRepository&#x3C;T,ID extends Serializable> Extends JpaRepository&#x3C;T,ID>{}</strong></code></pre>

### **두번째 순서 , 공통 메서드 만들기**

**어떤 Entity가 persistent Context에 존재하는지 체크하는 공통 메서드를 만들어보도록 하자.**

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p> <strong>어떤 Entity가 persistent Context에 존재하는지 체크한다.</strong></p></figcaption></figure>

### **세번째 순서 , 공통 메서드 구현체 만들기 :** SimpleJpaRepository 구현체와 우리가 정의한 MyRepository도 상속받은 인터페이스 생성

이 때는 굳이 접미어(Impl)을 지키지 않아도 된다.

**SimpleJpaRepository 구현체를 상속받은 뒤 우리가 정의한 MyRepository도 상속을 받아야 원하는 공통 기능을 추가할 수 있다.**

```
public interface SimpleMyRepository<T,ID extends Serializable>
Extends SimpleJpaRepository<T,ID> implements MyRepository<T,ID> {}
```

SimpleJpaRepository 구현체는 스프링 데이터 JPA가 제공하는 가장 기본적인 구현체이며 우리가 JPA 레파지토리를 상속받을 때 가져오는 구현체이다.

### **세번째 순서 , 공통 메서드 구현체에 생성자 추가하기**&#x20;

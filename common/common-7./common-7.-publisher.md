---
description: 스프링 데이터 Common 7. 스프링 데이터의 도메인 이벤트 Publisher
---

# 스프링 데이터 Common 7. 스프링 데이터의 도메인 이벤트 Publisher

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-27 Tue**

**last modified : 2022-09-27 Tue**

## \[1] 스프링 데이터의 이벤트 자동 퍼블리싱 기능

### 1) @DomainEvents와 @AfterDomainEventPublication

* **@DomainEvents** 어노테이션을 가지고 있는 메서드에 이벤트가 모아져있다.
* **@AfterDomainEventPublication**는 이벤트를 모두 보낸뒤 쌓여있던 이벤트를 비우는 작업을 한다.\
  (**메모리 누수방지**)

#### 즉 , 이벤트를 모아두는 클래스를 따로 만든 뒤 해당 클래스에 위 2개의 어노테이션을 붙이고 이벤트를 모아두는 곳과 지우는 곳을 직접 구현하여 만들 수 있다.

두 어노테이션의 활용 예시  : Repository를 Save할 때 엔티티에 쌓여있던 이벤트를 다 보내준다.

### &#x20;2) extends AbstractAggregateRoot

하지만 이를 직접 구현할 필요 없이 **스프링 데이터가 제공해주는 extends AbstractAggregateRoot** 를 통해 \
미리 구현되어 있는 기능을 사용한다.&#x20;

<figure><img src="../../.gitbook/assets/image (14) (2).png" alt=""><figcaption><p> <strong>스프링 데이터가 제공해주는 extends AbstractAggregateRoot</strong></p></figcaption></figure>

**AbstractAggregateRoot** 안에는 이미 위 2개의 어노테이션을 가지고 있는 메서드가 구현되어있다.

<figure><img src="../../.gitbook/assets/image (26) (1).png" alt=""><figcaption><p><strong>AbstractAggregateRoot</strong> 안  2개의 어노테이션</p></figcaption></figure>

이로써 직접 이벤트 퍼블리싱 기능을 구현하지 않아도 된다. **save할 때 이벤트를 만들어서 넣기만 하면 된다.**

## **\[2]** \[예시] publish 메서드 안의 이벤트 발생시키기

post에 publish 메서드를 만든 뒤 이 안에서 이벤트가 발생한다고 가정한다

### 1. publish 메서드 생성하기

### 2. 이벤트 생성 및 등록하기

<figure><img src="../../.gitbook/assets/image (22) (2).png" alt=""><figcaption><p> publish 메서드 생성 , 이벤트 생성 및 등록 </p></figcaption></figure>

일전에 만든 **PostPublishedEvent**의 인스턴스를 생성한 뒤

**AbstractAggregateRoot를 통해 registerEvent 메서드가 제공**되기에 만든 이벤트를 등록하도록 한다.

### 3. 이벤트 발생시키기

<figure><img src="../../.gitbook/assets/image (2) (1) (4).png" alt=""><figcaption><p> 이벤트 발생시키기</p></figcaption></figure>

이벤트를 만들어서 넣어주기만 하면 **save할 때** 자동으로 aggregate안에 모아져있던 이벤트를 모두 발생시킨다.&#x20;

#### 이벤트가 발생되는 구조&#x20;

일전에 만든 이벤트 리스너인 **postListener**가 빈으로 등록되어있기에 반응하여 동작하게 된다.

<figure><img src="../../.gitbook/assets/image (3) (3).png" alt=""><figcaption><p> <strong>postListener</strong></p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (25) (1).png" alt=""><figcaption><p> 빈으로 등록되어있는 postListener</p></figcaption></figure>

### 4. 테스트 결과

<figure><img src="../../.gitbook/assets/image (7) (4).png" alt=""><figcaption></figcaption></figure>






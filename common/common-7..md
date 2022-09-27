---
description: 스프링 데이터 Common이 제공하는  도메인 이벤트 퍼블리싱 기능 알아보기
---

# 스프링 데이터 Common 7. 도메인 이벤트

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-26 Mon**

**last modified : 2022-09-26 Mon**

## \[1] 도메인 이벤트 퍼블리싱이란?

* **도메인(엔티티)의 변화를 이벤트로 발생시켜주는 것**이다.
* 이 이벤트를 리스닝하는 **리스너**가 **도메인 엔티티 클래스의 변화를 감지**하고 어떠한 **이벤트를 기반으로 프로그래밍** 할 수 있다.
* 또한 도메인 이벤트를 직접 만들어 이벤트를 퍼블리싱하고 이벤트를 리스닝할 수 있다.

## \[2] 도메인 이벤트 간단하게 테스트하기

### 예시

* 사용자가 '**발행**' 기능을 수행할 때만 Post 도메인의 이벤트가 퍼블리싱 된다.
* 해당 이벤트가 퍼블리싱 되어 특정 프로그래밍을 구현하고자 한다.

### 1. 이벤트 만들기

**ApplicationEvent**를 상속받아 **PostPublishedEvent** 라는 클래스를 생성한다.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption><p><strong>ApplicationEvent</strong>를 상속 </p></figcaption></figure>

### 2. 이벤트를 발생시키는 곳이 Post임을 나타낸다.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

### 3. Post의 getter를 생성한다.

이 이벤트를 받는 리스너쪽에서 어떤 Post에 대한 이벤트였는지 참조할 수 있는 Getter를 만든다.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p> Post의 getter</p></figcaption></figure>

### 4. 이벤트 발생시키기

실제로 퍼블리싱하는 부분은 `applicationContext.publish(event);` 부분이다.

추후에는 실제로 Post에서 이벤트가 발생되도록 구현해볼 것이다.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### 5. 이벤트 리스너 생성

#### 이벤트 리스너 만드는 방법에는 2가지 방법이 있다.

#### 첫번째 방법 , ApplicationListener 클래스 상속받아 사용하기

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption><p> ApplicationListener 클래스 상속받아 사용하기</p></figcaption></figure>

**ApplicationListener** 클래스한테 이 이벤트 리스너가 받을 이벤트 타입을 알려주고 해당 클래스를 상속받는다.

이 이벤트 리스너가 이벤트를 리스닝했을 때 해야할일을 **onApplicationEvent** 메서드에서 구현한다.



##







**이 이벤트를**
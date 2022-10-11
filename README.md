---
description: '관계 설정하기 : 1대 다 맵핑'
---

# 1대 다 맵핑 관계 개념 및 구현하기

**기록 ✍️**

#### author : jung yuha

#### first registered : 2022-10-06 Thu

#### last modified : 2022-10-06 Thu

## \[1] 관계의 특징

* 관계에는 오직 **2개의 엔티티** 만이 참여한다.
* **단방향 관계**와 **양방향 관계** 모두 구현이 가능하다.
* 가장 두드러진 관계의 속성으로 **one to Many , Many to One** 가 있다.

## \[2] 단방향 Many to One의 기본 동작

### 1) 관계를 설정할 엔티티 생성하기

* Account라는 계정을 나타내는 엔티티와 Study 엔티티를 생성한다.

#### 1. Account 엔티티

* id , username , password 필드가 있다.
  * ![](<.gitbook/assets/image (13) (5).png>)

#### 2. Study 엔티티

* id , username , password 필드가 있다.
  * ![](<.gitbook/assets/image (16) (4).png>)

### 2) 관계 맺기

#### 1. Study 엔티티에 자기 주인을 Account 엔티티로 선언한다.

* 하나의 Account는 여러개의 Study를 가질 수 있다.\
  따라서 **Account 입장에서는 1대 다(one to many) 맵핑**이된다.
* 반대로 **Study 입장에서는 다 대 1 (many to one) 맵핑**이 된다.
  * ![](<.gitbook/assets/image (10) (5).png>)

#### 2. Study에 Account 를 지정하는 테스트 코드 작성하기

<figure><img src=".gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

#### 3. 테스트 결과

**Study라는 테이블 안에 Account 테이블의 PK값을 참조하는 FK 컬럼을 생성해서 가지고 있게 된다.**

**아래와 같이 owner\_id라는 FK가 생성되었다.**

![](<.gitbook/assets/image (44).png>)

### 3) Many to One 관계의 특징

* **이 관계에서의 주인은 Study가 주인이다. 반대쪽 (Account) 엔티티를 Study가 참조하고 있기 때문이다.**
* **Account 테이블엔 관계에 대한 정보가 없다.**
* '**주인**'이라 함은 관계를 설정했을 때 그 값을 반영하는 대상을 말한다.
  * 해당 예시는 Study와 Account의 관계를 설정했을 때 Study가 Account에 대해 셋팅을 했으므로 \
    Study가 주인이 된다.

## \[3] 단방향 One to Many의 기본 동작

### 1) 관계를 설정할 엔티티 생성하기

* Account라는 계정을 나타내는 엔티티와 Study 엔티티를 생성한다.(위와 동일하다.생략하겠다!)

### 2) 관계 맺기

* 이번에는 Account를 주체로 한다.즉 , Account가 자신이 만든 Study를 가지고 있다고 가정한다.

#### 1. Account 엔티티는  Study 엔티티를 여러개 가지고 있을 수 있으므로 Set 형태로  OneToMany로 선언한다.

<figure><img src=".gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

#### 2. 주인이 관계를 설정하도록 테스트 코드를 작성한다.

* 이번에는 Account가 관계를 가지고 있으므로 주인이 된다.

<figure><img src=".gitbook/assets/image (19) (4).png" alt=""><figcaption></figcaption></figure>

#### 3. 테스트 결과

* **Step1** - One to Many는 조인 테이블로 만들어진다. 따라서 총 3개의 테이블이 보여진다.
  * 첫번째 , account 테이블
    * ![](<.gitbook/assets/image (22) (1).png>)
  * 두번째 , account\_studies 테이블
    * ![](<.gitbook/assets/image (46).png>)
      * **account의 id와 study의 id 둘다 가지고 있다.**
  * 세번째 , study 테이블
    * ![](<.gitbook/assets/image (12) (3).png>)
* **Step 2** - 각 테이블 별로 **FK**가 설정된다.
  *

      <figure><img src=".gitbook/assets/image (18) (4).png" alt=""><figcaption></figcaption></figure>
* **Step 3** - account , study , account\_studies 테이블 순으로 저장된다.
  * ![](<.gitbook/assets/image (34).png>)

### 3) 단방향 One to Many 관계의 특징

* 기본적으로 One to Many는 조인 테이블로 만들어진다.
* **Study 테이블과 Account 테이블 모두 관계에 대한 정보가 없다.**
* **관계에 대한 정보는 account\_studies 테이블에 있다.**

## **\[4] 양방향 관계의 필요성**

#### 서로가 참조하고 싶을 때 양방향 관계를 구현한다.

### 1) 2개의 단방향 관계

**아래와 같은 경우는 양방향 관계가 아니라 두 개의 단방향 관계이다!!! 즉 , 서로 연관 관계가 없다.**

* &#x20;study쪽에서 오너 정보 (account)를 참조함과 동시에
  * ![](<.gitbook/assets/image (21) (2).png>)
* &#x20;account 에서도 order 정보를 참조할 때
  * ![](<.gitbook/assets/image (11) (5).png>)

### 2) 양방향 관계로 만들기

* one to many쪽에다가 mapped By 속성으로 이 관계가 반대쪽에 어떻게 맵핑이 되어있는지 정의한 필드를 적는다. \
  **그래야 Account쪽에서 필요한 관계를 부가적으로 만들지 않고  study에 정의되어 있는 관계에 종속된다.**
* 즉 , **주인은 Study 엔티티가 된다.**
  * study 쪽에서 owner라는 필드로 관계를 정의했으므로 one to many쪽에다가 mapped By 속성으로\
    owner 필드를 적는다.

<figure><img src=".gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

### 3) 양방향 관계의 특징과 주의해야할점

* **FK키를 가지게 되는 엔티티가 주인이 된다.** 해당 예시에서는 Study가 주인이 된다.
* ⭐️ ⭐️ **관계의 주인이 Study이므로 관계의 맵핑 주체는 Study가 된다.**
  * 그렇지 않으면 관계에 대한 정보가 저장되지 않는다.

#### \[예시] ⭐️⭐️ 관계의 맵핑 주체는 관계의 주인이 하지 않는 경우엔 관계에 대한 정보가 저장되지 않는다.

* &#x20;Account 엔티티가 맵핑의 주체가 되어 관계 정보가 저장되지 않는 예시이다.

**1-1.테스트 코드**

<figure><img src=".gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

**1-2. 테스트 결과**

* account 테이블엔 관계의 정보가 없다.
  * ![](<.gitbook/assets/image (6).png>)
* study 테이블의 FK값에도 정보가 들어오지 않는다.
  * ![](<.gitbook/assets/image (7) (1).png>)

**2-1.  관계의 주인인 Study가 관계를 맵핑하도록 테스트 코드 작성**

* 객체 지향 관점에서는 양쪽 모두 맵핑해야하므로 둘 다 동시에 맵핑하도록 한다.
* **account 쪽은 생략해도 별 일 없겠지만 객체 지향 관점을 염두에 두고 꼭 둘이 같이 묶어서 진행하도록 한다.**

<figure><img src=".gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

* account 쪽에 묶어서 구현하는 메서드를 만들어서 구현해도 좋다.
  *   add 메서드 구현하기

      <figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
  *   &#x20;remove 메서드 또한 같이 묶어서 구현해야한다.

      <figure><img src=".gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>
  *   테스트 코드

      <figure><img src=".gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

**2-2. 테스트 결과**

* study 테이블의 FK값에 정보가 들어온다.
  * ![](<.gitbook/assets/image (3) (1).png>)

****


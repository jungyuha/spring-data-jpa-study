---
description: hibernate에서 가장 중요한 엔티티 상태를 전이시키는 cascade 옵션에 대해서 살펴본다.
---

# 엔티티 상태와 Cascade

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-10-11 Tue**

**last modified : 2022-10-11 Tue**

## **이번 단원의 목표**

hibernate에서 가장 중요한 **엔티티 상태를 전이**시키는 **cascade** 옵션에 대해서 살펴본다.

## \[1] C**ascade란?**

### **1)** C**ascade란?**

#### 어떤 특정 엔티티 안에 있는 다른 엔티티에게도 상태 변화를 전파시키는 것이다.

아래 **** Study 엔티티의 상태가 A에서 B로 변할 때 Account 의 엔티티 상태도 같이 A에서 B로 변경시키고 싶을 때\
사용한다.

![](<../.gitbook/assets/image (23).png>)

### **2)** C**ascade 옵션을 설정하는 곳**

**엔티티**에 있는 **@OneToMany 또는 @ ManyToOne 어노테이션 옵션**에서 설정한다.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### 3) Cascade 옵션의 종류

#### 1. default

* 기본적으로 cascade 옵션은 아무것도 없다.(default)
* 따라서 cascade만 작성한경우 상태전이가 일어나지 않는다.

![](<../.gitbook/assets/image (9).png>)

## \[2] 엔티티의 '상태'

### 1) 엔티티 상태의 종류

#### 엔티티의 상태엔 크게 4가지가 있다. -> Transient , Persistent , Detached, Removed

#### Casecade는 이러한  4가지 상태들을 전이시킨다.

#### 1. Transient 상태

* 다음과 같이 new로 엔티티를 선언했을 때 엔티티 상태는 **Transient 상태**이다.
* Hibernate(JPA)가 이 엔티티 상태에 대해 **전혀 모른다**!!
* 해당 엔티티들은 데이터베이스에 맵핑되어있는 상태가 **아니다**.
* **즉 , 이 객체들은 데이터베이스에 반영이 될지 말지 아직 확정이 되지 않은 객체들이다.**

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

#### 2. Persistent 상태

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption><p> Persistent 상태</p></figcaption></figure>

* 저장(Save)을 하면 **Hibernate(JPA)**가 이 엔티티 상태에 대해 **아는 상태**가 된다.!!
* 하지만 저장(Save)을 했다고해서 해당 객체가 데이터베이스에 반영이 된 것은 아니다.
  * **Hibernate(JPA)**에서 **** Persistent 상태로 관리하고 있다가 데이터베이스에 Sync가 필요한 시점을 판단하고나서 반영한다.&#x20;
* 즉 ,  **Hibernate(JPA)**가 이 엔티티 상태에 대해 **알고 관리하는** 객체 상태가 된다.!!
* Persistent 상태에서는 **Hibernate(JPA)가 여러가지의 역할을 수행한다.**
  * **(역할 1) 1차 캐싱 : Session이라는 Persistent Context에 저장이 된 상태**를 의미한다.
    * session의 save를 통해 1차 캐싱이 된 상태에서 session.load를 통해 인스턴스를 요구하면\
      **select 쿼리가 발생하지 않는다. 이미 객체가 session에 캐싱되어있는 상태이기 때문이다.**
    *   **Save 메서드 , 즉 Insert 쿼리는 트랜잭션이 끝날 때인 제일 마지막에 발생한다.**

        <figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption><p><strong>1차 캐싱 : Session이라는 Persistent Context에 저장이 된 상태</strong></p></figcaption></figure>
  * **(역할 2) Dircy Checking : 객체의 상태 변화를 감시한다.**
    *   **다음과 같이 객체의 name을 설정하고 -> Save하고 -> 다시 다른 값으로 name을 설정한 경우 다시 Save를 하지 않아도 알아서 Update 쿼리를 수행한다.**

        <figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p> <strong>객체의 상태 변화를 감시</strong>   </p></figcaption></figure>
    *   위의 쿼리에서 여러번 바꿔 다시 처음 name의 값과 같아진다면 처음 상태와 같아지므로 Update 쿼리는 수행되지 않는다. 즉 , Persistent 상태에서 객체의 상태 변화를 감시하고 있음을 알 수 있다.

        <figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p> <strong>객체의 상태 변화를 감시</strong>   </p></figcaption></figure>

#### 3.  Detached 상태

* 트랜잭션이 끝나서 Session 밖으로 나온 상태이다.
* 예를들면 Service에서 Repository를 호출하여 **Repository에서 리턴한 객체를 사용하는 경우**에 \
  Repository의 트랜잭션은 끝이난 뒤 리턴한 것이므로 서비스에서 사용하는 **해당 객체는 Detached 상태가 된다.**&#x20;
* **Hibernate(JPA)**가 더 이상 관리하지 않는 객체이다.
* 만약 **Detached 상태가 된 객체**를 **Hibernate(JPA)**가 다시 관리하고자 한다면 Reattached를 해야한다.\
  다음은 Reattached시 사용되는 메서드이다.
  * ![](<../.gitbook/assets/image (13).png>)

#### 4. Removed 상태

*   Persistent상태를 Detached 상태로 만들려면 Session의 delete 메서드를 사용하면 된다.

    * 엔티티(객체)의 상태는 Removed 상태가 된다.



## \[3] Cascade 예시

### 1) 객체가 서로 Parent와 Child 관계를 가질 때

#### \[예시] Post 엔티티 , Comment 엔티티가 있고 Post엔티티의 저장/삭제시 Comment 엔티티도 따라서 저장/삭제될 수 있도록 만들어 보자

#### 1) Post 엔티티

* Child 엔티티로 Comments를 선언한다.
* Comment를 추가하는 메서드를 선언한다.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

#### 2) Comment 엔티티

![](<../.gitbook/assets/image (16).png>)

#### 3) Cascade 옵션 없이 Post 선언 및 Comment 추가 후 Post만 Save할 경우

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

#### 결과 : Post만 저장된다.

#### 4) Cascade 옵션으로 저장하는 Persist를 Comment 엔티티에도 전이시키는 경우

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

#### 결과 : Post가 참조하는 Comment 엔티티도 같이 Persistent 상태가 된다. 따라서 같이 저장된다.&#x20;

#### 5) Cascade 옵션으로 삭제하는 Remove를 Comment 엔티티에도 전이시키는 경우

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p> Cascade 옵션은 여러개 설정이 가능하다.</p></figcaption></figure>

*   delete를 호출하면 Post 객체의 상태는 removed가 되고 Comment 객체의 상태 또한 removed되어 같이 삭제된다.

    <figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

    #### 결과 : 같이 삭제된다.

    * #### &#x20;![](<../.gitbook/assets/image (1).png>)

### 2) Cascade 옵션은 여러개 설정이 가능하다.

다음과 같이 Cascade 옵션은 여러개 설정이 가능하다.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p> Cascade 옵션은 여러개 설정이 가능하다.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p> Cascade 옵션은 여러개 설정이 가능하다.</p></figcaption></figure>


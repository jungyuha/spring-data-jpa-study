---
description: 스프링 데이터 Common 5. 커스텀 리포지토리 만들기
---

# 스프링 데이터 Common 5. 커스텀 리포지토리 만들기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-21 wed**

**last modified : 2022-09-22 Thu**

## **\[1] 프로젝트 기본 셋팅하기**

### 1) 프로젝트 개발 환경 셋팅

* 가장 기본적으로 JPA , H2 데이터베이스 의존성을 추가한다.
*   pom.xml의 H2 Scope를 test로 바꿔준다.



    <figure><img src="../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>


* **application.properties** 파일에서 쿼리 출력에 관해 설정한다.(출력되는 쿼리를 편하게 보기 위해서이다.)
  *   이 때 테스트 실행의 경우 show-sql은 자동으로 true가 된다.&#x20;

      <figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
  * ![](<../.gitbook/assets/image (9).png>)

**application.properties는 resources 아래에 있다.**

* ㅇㅇ

### 2) Entity 생성

![](<../.gitbook/assets/image (3) (1).png>)

* 컬럼의 크기가 255자가 넘는 경우에는 **@Lob** 어노테이션을 달아주도록 한다.
* 날짜 위에는 **@Temporal** 어노테이션을 달아주도록 한다.
* getter , setter를 생성한다.

### 3) Repository 생성

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### 4) 테스트 코드 생성

*   메서드를 굳이 호출하지 않아도 일단 빈으로 잘 등록이 되는지 부터 확인하도록 한다.

    <figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption><p>빈으로 잘 등록이 되는지  확인</p></figcaption></figure>



## \[2] 커스텀한 Repository를 만드는 방법1 : 스프링 데이터 Repository에 커스텀한 Repository 기능 추가하기&#x20;

* 다른 어노테이션을 붙여서 쿼리를 정의할 수도 있지만 쿼리 메소드(쿼리 생성과 쿼리 찾아쓰기)로 해결이 되지 않는 경우 직접 코딩으로 구현 가능한 방법이다.&#x20;

### 첫번째 순서 , 순수한 POJO Repository 인터페이스를 만든다.

<figure><img src="../.gitbook/assets/image (6) (1).png" alt=""><figcaption><p> Repository 인터페이스 생성</p></figcaption></figure>

### 두번째 순서 , 원하는 메서드를 정의한다.

<figure><img src="../.gitbook/assets/image (13) (2).png" alt=""><figcaption><p> 메서드를 정의</p></figcaption></figure>

### 세번째 순서 , 해당 메서드의 구현체를 정의한다.

* 이 때 , 해당 인터페이스 이름 뒤에 '**Impl**' 을 붙인 **클래스** 파일에 구현체를 정의해야한다.
* JPQL을 써서 쿼리를 직접 작성한다.

```java
entityManager.createQuery("SELECT p FROM Post AS p", Post.class).getResultList();
```

<figure><img src="../.gitbook/assets/image (16) (2).png" alt=""><figcaption><p> 메서드의 구현체를 정의한다.</p></figcaption></figure>

### 네번째 순서 , Repository에 커스텀 Repository를 등록한다.

프로젝트 셋팅 단계에서 만든 PostRepository에 바로 위에서 만든 커스텀한 Repository를 등록한다.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p> 프로젝트 셋팅 단계에서 만든 PostRepository</p></figcaption></figure>

**위 코드에서 PostCutomRepository 도 상속받도록 한다.**

이렇게 되면 **** J**paRepository 인터페이스 기능을 사용함과 동시에 직접만든 PostCustomRepository의 기능도 사용할 수 있다.**

```java
public interface PostRepository extends JpaRepositoryPost,Long> , PostCutomRepository
{
}
```

### 테스트 코드 작성&#x20;

<figure><img src="../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p> 테스트 결과</p></figcaption></figure>



## \[3] 커스텀한 Repository를 만드는 방법2 : 스프링 데이터 Repository의 기본 기능을 커스텀하여 덮어쓰기

* 기존 스프링 데이터 Repository의 기본 기능을 직접 커스텀하여 사용하고싶은 경우 직접 코딩으로 구현 가능한 방법이다.&#x20;

### 첫번째 순서, 커스텀한 Repository에 스프링 데이터 Repository의 기본 메서드를 가져온다.

이 때 기본 메서드를 가져오면 인터페이스 내 같은 이름의 함수로 중복이 된다.\
이 때 **스프링 데이터 Jpa는 항상 커스텀하게 구현한 메서드를 우선순위로 실행한다.**

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption><p> 스프링 데이터 Repository의 delete 함수를 가져온 모습 </p></figcaption></figure>

### 두번째 순서, 가져온 기본 메서드를 직접 구현한다.&#x20;

<figure><img src="../.gitbook/assets/image (10) (2).png" alt=""><figcaption><p> delete 함수를 직접 구현한 모습</p></figcaption></figure>

### 세번째 순서 , Repository에 커스텀 Repository를 등록한다.

프로젝트 셋팅 단계에서 만든 PostRepository에 바로 위에서 만든 커스텀한 Repository를 등록한다.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p> 프로젝트 셋팅 단계에서 만든 PostRepository</p></figcaption></figure>

**위 코드에서 PostCutomRepository\<Post> 도 상속받도록 한다.**

```java
publicP interface PostRepository extends JpaRepositoryPost,Long> , PostCutomRepository
{
}
```

### 테스트 코드 작성

#### 테스트 코드 1  : 데이터 INSERT 후 DELETE하기

<figure><img src="../.gitbook/assets/image (6) (3).png" alt=""><figcaption><p> 데이터 INSERT후 DELETE 하기</p></figcaption></figure>

#### 테스트 결과  : 실제로 삽입과 삭제가 일어나지 않는다. TEST에서는 어차피 다시 ROLLBACK 처리 할것이기 때문이다.

#### 테스트 코드 2 : 데이터 INSERT 후 SELECT하고 DELETE 하기

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p> 데이터 INSERT 후 SELECT하고 DELETE 하기</p></figcaption></figure>

#### &#x20;테스트 결과 : INSERT를 해야 SELECT가 가능하기때문에 INSERT,SELECT는 처리되나 DELETE는 실행되지 않는다.어차피 롤백되기 때문이다.

{% hint style="info" %}
#### 만약에 DELETE까지 실행시키고 싶다면?

#### 👉 flush 메서드를 실행한다.

entity manager는 이미 해당 객체를 removed 상태로 변경시킨 상태이나

데이터베이스와는 Sync 되지 않은 상태에서 (어차피 롤백 처리 할 것이므로) **flush 메서드를 실행**시키면

**강제로 removed 상태의 객체를 데이터베이스와 Sync**를 한다.(= delete 쿼리를 실행한다.)


{% endhint %}

<figure><img src="../.gitbook/assets/image (3) (2).png" alt=""><figcaption><p> flush 메서드를 실행</p></figcaption></figure>

## \[4] 접미어 설정하기(Impl)

인터페이스 구현체를 만들 때 기본 접미어로 Impl을 사용한다.이 때 Impl말고 다른 단어로 대체할 수 있다.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>기본 접미어 Impl</p></figcaption></figure>

### 애플리케이션 메인에서 @EnableJpaRepositories 재설정한다.

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption><p> Impl을 Default로 설정</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption><p>  Impl을 Default로 바꾼 모습</p></figcaption></figure>

---
description: Repository를 색다른 방법으로 정의해보기
---

# 스프링 데이터 Common 2. 인터페이스 정의하기

**기록 ✍️**

#### author : jung yuha

#### first registered : 2022-09-19 Mon

#### last modified : 2022-09-19 Mon

## \[1] 기존 Repository 작성 방법

#### spring data jpa 또는 스프링 데이터 Common이 제공하는 인터페이스를 상속받아서 제공되는 다양한 기능을 이용했다.

## \[2] **Repository 인터페이스로 공개할 메소드를 직접 일일히 정의하기**

* 위처럼 상속받은 인터페이스의 모든 기능을 제공받는 것 보다 내가 원하는 메서드만 구현하고 싶은 경우에 해당 방법을 사용한다.

1.  Repository 인터페이스를 만든다.



    <figure><img src="../.gitbook/assets/image (10) (2) (1).png" alt=""><figcaption><p> Repository 인터페이스 생성</p></figcaption></figure>
2. 아무 인터페이스를 상속받지 않고 **@RepositoryDefinition**을 사용해서 직접 정의한다.
   1.  첫번째 단점은 정말 아무 기능도 안 들어온다는 것이다.😅&#x20;

       <figure><img src="../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption><p> <strong>@RepositoryDefinition</strong>을 사용해서 직접 정의한다.</p></figcaption></figure>
   2.  두번째 단점은 하나하나 정의한 메서드를 테스트 해주어야한다.&#x20;

       <figure><img src="../.gitbook/assets/image (5) (1) (2).png" alt=""><figcaption><p> 테스트 코드 작성</p></figcaption></figure>
   3.  세번째 단점은 각 엔티티마다 공통된 메서드를 하나하나 모두 구현해주어야한다는 것이다.

       따라서 공통된 기능을 구현하는 공통 인터페이스를 정의하는 방법이 있다.

## \[3] 공통 인터페이스 정의하기

* 공통으로 구현되는 기능들을 모은 Repository를 만들 수도 있다.

### 공통 인터페이스 만들기

**Repository 인터페이스를 상속**받고 **@NoRepositoryBean 어노테이션**을 붙인다.

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption><p> 공통으로 구현되는 기능들을 모은 MyRepository</p></figcaption></figure>

그리고나서 **** 기존에 사용했던 **@RepositoryDefinition을 지우고** MyRepository 인터페이스를 상속받는다.

<figure><img src="../.gitbook/assets/image (6) (2) (1).png" alt=""><figcaption><p> <strong>@RepositoryDefinition을 지우고</strong> MyRepository 인터페이스를 상속받는다.</p></figcaption></figure>

공통 인터페이스에 CrudRepository의 특정 기능만 가져다 쓰고 싶으면 그냥 복사해서 가져다쓰면 된다.

<figure><img src="../.gitbook/assets/image (13) (1) (1).png" alt=""><figcaption><p> CrudRepository의 count 메서드를 복사한 모습</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (2) (2).png" alt=""><figcaption><p>테스트 코드</p></figcaption></figure>

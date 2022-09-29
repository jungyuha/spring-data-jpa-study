# Page 2

Extension을 만들어서 Extension 내에서 QueryDslRepository를 상속받아 QueryDSL을 사용하는 방





## \[1] Extension 만들기&#x20;

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## \[2] QueryDslRepository를 상속받기&#x20;

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption><p> QueryDslRepository를 상속받기</p></figcaption></figure>

## \[3] 구현체 안에서 QueryDSL을 사용한다.

구현체 안에서 QueryDslRepository를 상속받아 직접 QueryDSL을 사용한다.

이를 통해 복잡한 쿼리를 구현할 수 있다.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### 0) 사전작업

#### 1 . 엔티티 생성 : ook이라는 엔티티를 생성한다.

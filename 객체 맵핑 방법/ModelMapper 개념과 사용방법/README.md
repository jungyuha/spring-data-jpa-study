---
description: ModelMapper 활용법
---

# ModelMapper 개념과 사용방법

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-26 Mon**

**last modified : 2022-09-26 Mon**

## \[1] ModelMapper 정의와 특성

### ModelMapper란? <a href="#sub-title1-0" id="sub-title1-0"></a>

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>엔티티와 DTO간의 변환시 자동 맵핑한다.</p></figcaption></figure>

ModelMapper는 엔티티와 DTO 간에 변환할 때 자동으로 매핑시켜주는 라이브러리이다.

매핑해줄 클래스에는 setter가 있어야 하고 매핑이 되는 클래스에서 getter가 있어야 사용 가능하다.

기본적으로 ModelMapper에서 제공해주는 map메서드를 이용하여 변환하는데 이를 통해 클래스 내부에 있는 변수들의 이름을 분석하여 자동 매핑을 수행할 수 있다.

#### modelMapper의 탄생 배경

* Entity를 View Rayer까지 사용하게되면 양방향으로 연결된 엔티티는 순환 참조 문제가 발생할 수 있다.
* 또한 다른 Entity를 참조하고 있는 경우 현재 Entity 뿐만 아니라 다른 Entity에도 원치 않는 변경이 일어나거나, 무거운 양의 데이터를 들고 여러 영역을 오가는 것이 성능 상에도 좋지 않다.
* &#x20;dto에는 데이터베이스에 들어있는 값 이외에도 다양하고 수많은 변수들로 구성되어있기 때문에 DB 레이어와 view 레이어는 분류할 필요가 있다.

**따라서 DB Layer에는 Entity, View Layer에서는 DTO를 사용하여 역할을 분리하여 사용하도록 하며 이를 도와주는 라이브러리가 `modelMapper`인 것이다.**

#### modelMapper 구현 예시

**예시 1**&#x20;

* DB와 mapping되고 데이터의 저장, 수정, 조회, 삭제에 대해 직접 DB와 커넥션하며 데이터를 관리하고 보관하는 Persistence Layer까지는 Entity를 사용한다.
* Service 영역에서부터 클라이언트 영역까지는 DTO를 사용한다.
* Service 영역에서 DTO와 Entity가 바뀐다.

## \[2] ModelMapper 사용해보기

### 1) ModelMapper 의존성 추가 <a href="#sub-title1-0" id="sub-title1-0"></a>

ㅇㅇ

### 2) ModelMapper 등록하기

{% hint style="info" %}
**modelMapper를 등록하는 방법**으로는 아래와 같이 두가지가 있다.

* 첫번째 방법 , 사용되는 클래스마다  직접 ModelMapper 인스턴스를 생성하기
* 두번째 방법 , 빈으로 등록하여 사용되는 클래스마다 등록된 빈을 호출하기
{% endhint %}

나는 두번째 방법을 선택해서 구현해보고자 한다.

#### DB설정 파일인 RootContext에 다음과 같이 ModelMapper을 빈으로 등록한다.

```java
public class RootContext {
	@Bean
	public ModelMapper modelMapper() {
		return new ModelMapper();
	}
    
	...
}
```

#### 또는 DB설정 파일인 RootContext에 다음과 같이 ModelMapper을 전략에 따라 구분하여 빈으로 등록한다.

```java
@Configuration
public class CustomModelMapper {
    @Bean
    public ModelMapper strictMapper() {
        // 매핑 전략 설정
        ModelMapper modelMapper = new ModelMapper();
        modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT);
        return modelMapper;
    }
    @Bean
    public ModelMapper standardMapper() {
        // 매핑 전략 설정
        ModelMapper modelMapper = new ModelMapper();
        modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STANDARD);
        System.out.println(modelMapper.getConfiguration().getMatchingStrategy().toString());
        return modelMapper;
    }
    @Bean
    public ModelMapper looseMapper() {
        // 매핑 전략 설정
        ModelMapper modelMapper = new ModelMapper();
        modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.LOOSE) ;
        return modelMapper;
    }
}
```

### 3) 사용 예시

#### 예시 1

```java
//DI
@Autowired
CustomModelMapper customModelMapper;
//사용부분
ArrayList<AfterVo> afterVoList new ArrayList<AfterVo>();
ModelMapper modelMapper
for (BeforeVo v : BeforeVoList) {
customModelMapper.strictMapper();
afterVoList.add(modelMapper.map(v, AfterVo.class));
```

만약 BeforeVo서 id, name, address, phone 4가지 필드를 가지고, AfterVo에는 id,name 2가지만 가지고 있다면이때 **BeforeVo의 id, name만 AfterVo에 맞게 매핑**된다.

#### 예시 2

```java
public class AServiceImpl implements AService {
	@Autowired
	ModelMapper modelMapper;
	public int methodA() {
		modelMapper.map(source, destination);
	}
	...
}
```

## \[3] ModelMapper 매핑 전략 3가지

### 1) MatchingStrategies.STANDRD

* 지능적으로 매핑한다.
* source 속성을 destination 속성과 지능적으로 일치시킬 수 있으므로, 모든 destination 속성이 일치하고 모 든 source 속성 이름에 토큰이 하나 이상 일치해야한다.
* &#x20;토큰을 어떤 순서로도 일치시킬 수 있다.
* 모든 destination 속성들이 매치 되어야 한다.
* 모든 source 속성은 최소 하나 이상의 이름이 매치가 되어야 한다.&#x20;

### 2) MatchingStrategies. STRICT

* 정확히 일치하는 필드만 매핑한다.
* Strict source의 속성과 destination 속성의 이름이 정확히 일치 할때만 매핑한다.

### 3) Matching Strategies.LOOSE

* 느슨하게 매핑한다.
* 계층 구조의 마지막 destination 속성만 일치하도록하여 source 속성을 destination 속성에 일치시킨다.
* 토큰을 어떤 순서로도 일치시킬 수 있다.
* 마지막 detination 속성 이름은 모든 토큰이 일치해야 한다.
* 마지막 source 속성 이름에는 일치하는 토큰이 하나 이상 있어야 한다.

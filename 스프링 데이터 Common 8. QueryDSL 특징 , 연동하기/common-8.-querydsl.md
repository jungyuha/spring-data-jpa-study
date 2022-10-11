---
description: 기본 리포지토리 커스터마이징 안 하고 기본 JpaRepository를 상속받은 레포지토리에 QueryDSL을 구현하는 방법
---

# 스프링 데이터 Common 8. QueryDSL 기본 구현하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-28 Wed**

**last modified : 2022-09-29 Thu**

## \[1] 레파지토리에 **QuerydslPredicateExecutor 추가**

일전에 만든 레파지토리에 QuerydslPredicateExecutor를 추가한다.

```java
public interface AccountRepository extends JpaRepository<Account, Long>
, QuerydslPredicateExecutor<Account> {
}

```

QuerydslPredicateExecutor를 추가하면 **QuerydslPredicateExecutor**가 제공하는 **** 다음 2가지 메서드를\
사용할 수 있다.

![](<../.gitbook/assets/image (5) (2).png>)

#### **QuerydslPredicateExecutor**가 제공하는 **** 메서드

<figure><img src="../.gitbook/assets/image (4) (6).png" alt=""><figcaption><p> <strong>QuerydslPredicateExecutor</strong>가 제공하는 <strong></strong> 메서드</p></figcaption></figure>

## \[2] queryDSL 기본 활용 방법

테스트 코드에 queryDSL을 작성해본다.

#### 1. 쿼리 생성

#### \[예시] account 엔티티의 firstName이 containsIgnore로 'keesun'를 가짐과 동시에 lastName이 startWith로 'balk'을 가지는 쿼리를 생성해본다.

<figure><img src="../.gitbook/assets/image (16) (2) (2).png" alt=""><figcaption></figcaption></figure>

#### 2. findOne 메서드로 predicate를 넣어 쿼리 Language를 만든다.&#x20;

<figure><img src="../.gitbook/assets/image (18) (1).png" alt=""><figcaption><p> 테스트 코드</p></figcaption></figure>

## \[3] queryDSL로 동적 쿼리 작성하기

실제로 연습 프로젝트에 queryDSL로 동적 쿼리를 생성해 적용해보았다.&#x20;

#### **\[예시] 다음 조건을 만족하는 동적 쿼리를 QueryDSL로 구현해보자!** **- UserInfoVo에서 userType이 존재하는 경우에 equalsIgnoreCase 검색조건으로 설정하고**    **없으면 설정하지 않는다.** **- UserInfoVo에서 bizName이 존재하는 경우에 equalsIgnoreCase 검색조건으로 설정하고**    **없으면 설정하지 않는다.** **- UserInfoVo에서 아무런 조건이 설정되어있지 않으면 전체 검색을 한다.**

### **1)** BooleanBuilder를 선언한다.

```java
public List<User> getUserList(UserInfoVo userInfoVo) {
	
	QUser user = QUser.user;
	List<User> userList =  new ArrayList<User>();
	BooleanBuilder builder = new BooleanBuilder(); // BooleanBuilder를 선언한다.
}
```

### 2) 각 적용할 검색 조건을 predicate 형식으로 만든다.

### 3) 만들어진 predicate를 조건에 만족할 때마다 BooleanBuilder에 적용한다.

```java
public List<User> getUserList(UserInfoVo userInfoVo) {
	
	QUser user = QUser.user;
	List<User> userList =  new ArrayList<User>();
	BooleanBuilder builder = new BooleanBuilder(); // BooleanBuilder를 선언한다.
	
	// 검색 조건 1. userType : 사용자 / 관리자
	if(userInfoVo.getUserType() != null ) {
		// 검색 조건을 predicate 형식으로 만든 뒤 BooleanBuilder에 적용
		builder.and(user.userType.equalsIgnoreCase(userInfoVo.getUserType().getCode()));
	}
	// 검색 조건 2. bizName : 사업장명
	if(userInfoVo.getBizName() != null ) {
		// 검색 조건을 predicate 형식으로 만든 뒤 BooleanBuilder에 적용
		builder.and(user.bizName.containsIgnoreCase(userInfoVo.getBizName()));
	}
}
```

### 4) 완성된  BooleanBuilder를 Repository 매개변수 값으로 넣어준다.

```java
public List<User> getUserList(UserInfoVo userInfoVo) {
	
	QUser user = QUser.user;
	List<User> userList =  new ArrayList<User>();
	BooleanBuilder builder = new BooleanBuilder(); // BooleanBuilder를 선언한다.
	
	// 검색 조건 1. userType : 사용자 / 관리자
	if(userInfoVo.getUserType() != null ) {
		// 검색 조건을 predicate 형식으로 만든 뒤 BooleanBuilder에 적용
		builder.and(user.userType.equalsIgnoreCase(userInfoVo.getUserType().getCode()));
	}
	// 검색 조건 2. bizName : 사업장명
	if(userInfoVo.getBizName() != null ) {
		// 검색 조건을 predicate 형식으로 만든 뒤 BooleanBuilder에 적용
		builder.and(user.bizName.containsIgnoreCase(userInfoVo.getBizName()));
	}
	
	try {
		// 완성된  BooleanBuilder를 Repository 매개변수 값으로 넣어준다.
    		userList = ImmutableList.copyOf(userRepository.findAll(builder));
    		return userList;
	}
	catch (Exception e) {
		throw new RuntimeException(e);
	}
}
```

### \[완성 코드]

<pre class="language-java"><code class="lang-java">import com.google.common.collect.ImmutableList;
import com.querydsl.core.BooleanBuilder;
...
<strong>
</strong><strong>public List&#x3C;User> getUserList(UserInfoVo userInfoVo) {
</strong>	
	QUser user = QUser.user;
	List&#x3C;User> userList =  new ArrayList&#x3C;User>();
	BooleanBuilder builder = new BooleanBuilder(); // BooleanBuilder를 선언한다.
	
	// 검색 조건 1. userType : 사용자 / 관리자
	if(userInfoVo.getUserType() != null ) {
		// 검색 조건을 predicate 형식으로 만든 뒤 BooleanBuilder에 적용
		builder.and(user.userType.equalsIgnoreCase(userInfoVo.getUserType().getCode()));
	}
	// 검색 조건 2. bizName : 사업장명
	if(userInfoVo.getBizName() != null ) {
		// 검색 조건을 predicate 형식으로 만든 뒤 BooleanBuilder에 적용
		builder.and(user.bizName.containsIgnoreCase(userInfoVo.getBizName()));
	}
	
	try {
		// 완성된  BooleanBuilder를 Repository 매개변수 값으로 넣어준다.
    		userList = ImmutableList.copyOf(userRepository.findAll(builder));
    		return userList;
	}
	catch (Exception e) {
		throw new RuntimeException(e);
	}
}</code></pre>


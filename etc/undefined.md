# 상속 관계 매핑 전략 구현하기 (미완성)

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-10-20 Thu**

**last modified : 2022-10-20 Thu**

**출처 :** [**https://ict-nroo.tistory.com/128**](https://ict-nroo.tistory.com/128)****

## \[1] 상속 관계 매핑 전략

### 1) 상속관계 매핑이란?

* &#x20;**객체의 상속 구조**와 **DB의 슈퍼타입 서브타입 관계**를 매핑하는 것이다.
  * 객체는 상속관계가 존재하지만, 관계형 데이터베이스는 상속 관계가 없다.



## \[2] 구현할 엔티티의 상속 관계

* **부모** 클래스명 : AA
* **자식** 클래스명 : aa
* **aa 클래스는 AA 클래스의 모든 속성을 상속받는다.**

## \[2] 구현

### 1) 부모 클래스 생성 코드

* 부모 클래스인 **AA 클래스**를 생성한다.
* **핵심 포인트**
  1. &#x20;**InheritanceType.JOINED  어노테이션**
  2. **DiscriminatorColumn 어노테이션**

```java
import java.time.LocalDateTime;
import java.util.Date;

import javax.persistence.*;

import org.hibernate.annotations.CreationTimestamp;

import com.onlinepharm.epms.util.code.UserType;

import lombok.Data;

@Entity
@Inheritance(strategy = InheritanceType.JOINED) // (1) InheritanceType.JOINED 선언
@DiscriminatorColumn(name = "dtype")	// (2) DiscriminatorColumn 설정
@Data
public abstract class AA {
	
	@Id @GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long aId;

	private String password;
		
	private String aName;
	
	@Enumerated(EnumType.STRING)  
	private UserType userType;
	
	@CreationTimestamp
	private LocalDateTime rgstDate;
	
	@CreationTimestamp
	private LocalDateTime modiDate ;

}

```

### 2) 자식 클래스 생성 코드

* 자식 클래스인 **aa 클래스**를 생성한다.
* **핵심 포인트**
  1. &#x20;**부모 클래스 AA 상속**
  2. **DiscriminatorValue 어노테이션**

```java
import javax.persistence.*;

@Entity
@DiscriminatorValue("aa")	// (2) DiscriminatorValue 어노테이션
@Data
public class aa extends AA { // (1) 부모 클래스 AA 상속
	
	private String mobileNumber;
	
	private String ci;
	
	private String aaId;
    
}

```

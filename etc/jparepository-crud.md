---
description: JpaRepository로 기본 CRUD 구현하기
---

# JpaRepository로 기본 CRUD 구현하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-27 Tue**

**last modified : 2022-09-27 Tue**

## \[1] JpaRepository 인터페이스 상속 받기

### **1) Repository 인터페이스 생성**

JpaRepository를 상속 받아 Entity 클래스의 기본적인 `CRUD`를 구현해보도록 한다. ****&#x20;

```java
public interface 인터페이스명 extends JpaRepository <엔티티 클래스명, PK타입>
```

## \[**2**] INSERT

JpaRepository의 **Save 메서드**를 사용한다.

```java
@Autowired
private UserRepository userRepository;

@Test
public void create(){

    User user = new User();
    user.setUserId("yuha123");
    user.setName("yuha");
    user.setEmail("yoohaJpaTest@gmail.com");
    user.setPhoneNumber("01012341234");
    user.setCreatedAt(LocalDateTime.now());

    User newUser = userRepository.save(user);
}
```

## \[3] SELECT

* JpaRepository의 **find\* 메서드**를 사용한다.
* Optional의 **ifPresent 메서드**를 활용해 존재여부를 판단한다.
  * Optional 객체가 감싸고 있는 값이 존재할 경우에만 실행 로직을 함수형 인자로 넘긴다.

```java
@Autowired
private UserRepository userRepository;

@Test
public void select(){
   Optional<User> user = userRepository.findById('yuha123');

    user.ifPresent(selectedUser->{
        System.out.println("user : " + selectedUser);
    });
}
```



## \[4] UPDATE

* JpaRepository의 **find\* 메서드**와 **save 메서드**를 활용한다.
* Optional의 **ifPresent 메서드**를 활용해 존재여부를 판단한다.
  * Optional 객체가 감싸고 있는 값이 존재할 경우에만 실행 로직을 함수형 인자로 넘긴다.
* 조회된 객체 selectedUser에서 다시 **set한 인자값만** 업데이트된다.
  * 즉 아래는 updatedAt과 phoneNumber만 변경된다.

```java
@Autowired
private UserRepository userRepository;

@Test
public void update(){
    Optional<User> user = userRepository.findById('yuha123');

    user.ifPresent(selectedUser->{
       selectedUser.setPhoneNumber("01043214321");
       selectedUser.setUpdatedAt(LocalDateTime.now());
       
       userRepository.save(selectedUser);
    });
}
```



## \[5] DELETE

* JpaRepository의 **find\* 메서드**와 **delete 메서드**를 활용한다.
* 데이터가 존재해야지 삭제가 가능하기에 Optional의 **ifPresent 메서드**를 활용해 존재여부를 판단한다.
  * Optional 객체가 감싸고 있는 값이 존재할 경우에만 실행 로직을 함수형 인자로 넘긴다.
* Optional의 **isPresent() 메서드**를 활용해 데이터가 삭제되었는지 확인한다.

```java
@Autowired
private UserRepository userRepository;

@Test
public void delete(){
    Optional<User> user = userRepository.findById('yuha123');

    user.ifPresent(selectUser->{
        userRepository.delete(selectUser);
    });

    //삭제가 제대로 이뤄졌는지 확인
    Optional<User> deleteUser = userRepository.findById('yuha123');

    //데이터가 삭제됐다면 false가 나온다.
    Assert.assertFalse(deleteUser.isPresent());
}
```

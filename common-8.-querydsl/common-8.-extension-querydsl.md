---
description: 스프링 데이터 Common 8. Extension으로 QueryDSL 구현하기
---

# 스프링 데이터 Common 8. Extension으로 QueryDSL 구현하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-29 Thu**

**last modified : 2022-09-29 Thu**

## 이번 단원의 목표

* **Extension을 통해 직접 QueryDSL을 구현하여 복잡한 쿼리를 생성해본다.**

## \[1] Extension 만들기&#x20;

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

## \[2] QueryDslRepository를 상속받기&#x20;

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption><p> QueryDslRepository를 상속받기</p></figcaption></figure>

## \[3] 구현체 안에서 QueryDSL을 사용한다.

구현체 안에서 QueryDslRepository를 상속받아 직접 QueryDSL을 사용한다.

이를 통해 복잡한 쿼리를 구현할 수 있다.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

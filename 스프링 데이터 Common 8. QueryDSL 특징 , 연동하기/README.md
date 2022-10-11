---
description: 스프링 데이터 Common 8. QueryDSL 특징 , 셋팅하기
---

# 스프링 데이터 Common 8. QueryDSL 특징 , 연동하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-09-28 Wed**

**last modified : 2022-09-28 Wed**

## \[1] QueryDSL의 특징 &#x20;

* 조건문을 표현하는 방법이 타입 세이프하다.
* 조건문들을 조합할 수 있다.
* 쿼리를 자바 코드로 짤 수 있다.
  * ![](<../.gitbook/assets/image (32).png>)
* **Predicate 인터페이스**로 **** 조건문들을 표현한다.
  * **Predicate 인터페이스는 서로** 조합할 수도 있고 또는 별도의 클래스에 모아서 따로 관리할 수도 있다.
* 조건이 많아질수록 쿼리메서드가 굉장히 복잡해지는데 이 때 QueryDSL을 쓰면 된다.
* 주로 아래 제공되는 2개의 인터페이스가 많이 쓰인다.
  * Optional findOne(**Predicate**): 이런 저런 조건으로 무언가 하나를 찾는다.
    * Optional을 리턴한다.
  * List|Page|.. findAll(**Predicate**): 이런 저런 조건으로 무언가 여러개를 찾는다.
    * 리턴타입이 여러가지이다.(List , Page ,Iterable ...)&#x20;
    * Page를 리턴받고 싶으면 매개변수에 Pageable을 넣어주어야한다.
* 즉, 쿼리 메서드명을 늘리지 않아도 **Predicate 인터페이스**의 **** findOne과 findAll을 활용해 쿼리를 만들어 쓸 수 있다.

## \[2] QueryDSL 셋팅하기

### 0) 사전작업

#### 1 . 엔티티 생성 : Account 라는 엔티티를 생성한다.

![](<../.gitbook/assets/image (19) (3).png>)

#### 2. 레파지토리 생성 : JpaRepository를 상속받아 AccountRepository를 생성한다.

<figure><img src="../.gitbook/assets/image (9) (1) (3).png" alt=""><figcaption><p> AccountRepository</p></figcaption></figure>

#### 3. 테스트 파일 생성

![](<../.gitbook/assets/image (23) (2) (1).png>)

### 1) 의존성 추가

#### 1. 의존성 추가하기

#### Maven : pom.xml

```xml
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-apt</artifactId>
        </dependency>
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-jpa</artifactId>
        </dependency>
```

#### Gradle : build.gradle

<pre class="language-properties"><code class="lang-properties">buildscript {
    ext {
        queryDslVersion = "5.0.0"
    }
}

<strong>dependencies {
</strong>    implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"
    implementation "com.querydsl:querydsl-apt:${queryDslVersion}"
    //...
}</code></pre>

* queryDSL은 스프링부트가 의존성 관리를 해준다.따라서 pom.xml에 따로 버전을 명시하지 않아도 된다.
  * <img src="../.gitbook/assets/image (30) (1).png" alt="" data-size="original">
    * **query-apt 모듈**은 쿼리를 생성해준다. :  엔티티 모델을 보고 그 모델에 맞는 **쿼리 Language를 만든다.**
      * 이를 위해서는 **플러그인 설정이 필요**하다.&#x20;

#### 2. 플러그인 설정하기

#### Maven : pom.xml

* **querydsl.apt.jpa.JPAAnnotationProcessor**를 사용할 것임을 명시한다. ****&#x20;

```xml
<plugin>
    <groupId>com.mysema.maven</groupId>
    <artifactId>apt-maven-plugin</artifactId>
    <version>1.1.3</version>
    <executions>
        <execution>
            <goals>
                <goal>process</goal>
            </goals>
            <configuration>
                <outputDirectory>target/generated-sources/java</outputDirectory>
                <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
            </configuration>
        </execution>
    </executions>
</plugin>
```

#### Gradle : build.gradle

```properties
buildscript {
	dependencies {
		classpath("gradle.plugin.com.ewerk.gradle.plugins:querydsl-plugin:1.0.10")
	}
}

apply plugin: "com.ewerk.gradle.plugins.querydsl"

dependencies {
	//..
	implementation 'com.querydsl:querydsl-jpa'
	implementation 'com.querydsl:querydsl-apt'
}

//querydsl 추가
def querydslDir = "src/main/generated/queryDsl"
querydsl {
   library = "com.querydsl:querydsl-apt"
   jpa = true
   querydslSourcesDir = querydslDir
}
sourceSets {
   main {
      java {
         srcDirs = ['src/main/java', querydslDir]
      }
   }
}
compileQuerydsl{
   options.annotationProcessorPath = configurations.querydsl
}
configurations {
   querydsl.extendsFrom compileClasspath
}
```

* 이 플러그인은 버전관리가 되지 않기 때문에 버전을 직접 명시해야한다.
* 자동생성되는 클래스들은 **target/generated-sources/java** 밑에 두도록 설정한다.
* 이클립스에서 Q클래스 Import 안되는 오류 해결 : [https://pamyferret.tistory.com/11](https://pamyferret.tistory.com/11)

#### 3. compile 실행

#### Gradle

* Gradle tasks의 메뉴에 들어가 **build의 오른쪽 버튼을 클릭해 Run gradle Tasks를** 실행한다.

![](<../.gitbook/assets/image (20) (2).png>)

![](<../.gitbook/assets/image (2) (3).png>)

**build/generated-sources/querydsl** 밑에 Q로 시작하는 **클래스가 자동생성**된다.\
**이는 각 엔티티에 대한 쿼리 Language를 만들어줄 것이다.**

#### 메이븐

* Maven의 lifecycle 메뉴에서 컴파일을 실행한다.
  * ![](<../.gitbook/assets/image (6) (2).png>)
* **target/generated-sources/java** 밑에 QAccount라는 **클래스가 자동생성**된다.\
  **이는 Account 엔티티에 대한 쿼리 Language를 만들어줄 것이다.**
  * ![](<../.gitbook/assets/image (11) (5) (1).png>)

### 3) 자동생성된 클래스 테스트

테스트 코드에 클래스가 잘 생성되는지 확인한다.

![](<../.gitbook/assets/image (8) (2).png>)


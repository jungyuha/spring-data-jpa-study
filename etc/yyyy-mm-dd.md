# \[날짜조회]yyyy-MM-dd 형태의 날짜 조회하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-11-18 Fri**

**last modified : 2022-11-18 Fri**

### 1) 현재 상태

* &#x20;애플리케이션 내에서는 yyyy-MM-dd 형태의 String 형식으로 되어있다.
  * 검색 시작 날짜 : `start_dt`_(YYYY-MM-DD)_
  * 검색 시작 날짜 : `end_dt`_(YYYY-MM-DD)_
* DB 내에서는 Timestamp 형식의 날짜가 저장되어있다.

### 2) 쿼리 빌더 생성하기

```java
LocalDate startDate = LocalDate.parse(start_dt, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
LocalDate endDate = LocalDate.parse(end_dt, DateTimeFormatter.ofPattern("yyyy-MM-dd"));

if (!Strings.isNullOrEmpty(start_dt) && )!Strings.isNullOrEmpty(end_dt) {
    builder.and(qTest.registerdTime.between(startDate, endDate));
} else {
    // default : toDay >=
    LocalDate currentDate = LocalDate.now();
    builder.and(qTest.registerdTime.gt(currentDate.atStartOfDay()));
}

```

### 3) 날짜 타입 parser 만들기 (string => LocalDateTime)

#### 1. Parser 생성하기

```java
@Getter
public class LocalDateParser {
    private LocalDate searchParamDt;

    public LocalDateParser(String currentDate) {
        this.searchParamDt = LocalDate.parse(currentDate, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
    }

    // 해당날짜의 시작시간(ex:2022-11-18 00:00:00)
    public LocalDateTime startDate() {
        return this.searchParamDt.atStartOfDay();
    }

    // 해당날짜의 끝 시간(ex:2022-11-18 23:59:59)
    public LocalDateTime endDate() {
	return LocalDateTime.of(this.searchParamDt, LocalTime.of(23,59,59));
    }
}
```

#### 2. Parser 이용하기

```java
LocalDateParser localDateParser = new LocalDateParser("2022-11-18");

builder.and(qTest.registerdTime.between(localDateParser.startDate(), localDateParser.endDate()));
```

### 4) String 타입 yyyy-MM-dd HH:mm:ss:SSS 만들기 (LocalDateTime => string)

#### 1.변환하기

```java
LocalDateTime pharmMdcnCpltDttm ; // 2022-11-25T15:01:56
String pharmMdcnCpltDttmDesc = pharmMdcnCpltDttm.format(
                DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")
    	        ); //2022-11-25 15:01:56
```

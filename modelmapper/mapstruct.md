---
description: MapStruct의 개념과 사용법
---

# MapStruct의 개념과 사용법

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-10-06 Thu**

**last modified : 2022-10-06 Tue**

## \[1] 개념

### MapStruct과 ModelMapper의 공통점

* &#x20;Entity와 Dto간의 매핑을 지원하는 라이브러리다.
* getter/setter를 통해 Entity와 Dto간의 매핑을 직접 구현한다.

### MapStruct과 ModelMapper의 차이점

* MapStruct는 컴파일시 미리 생성된 구현체를 통해 Mapping하기에 ModelMapper보다 훨씬 빠르다.
* ModelMapper는 맵핑시 리플렉션이 발생한다.

## \[2] 적용

### 1) 의존성 추가하기

* 주의해야할 점 : 순서대로 기재해야한다!!

```properties
annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.1.Final'
implementation 'org.mapstruct:mapstruct:1.4.1.Final'
```

#### 순서를 지키지 않으면 다음과 같은 오류가 난다.

```
- handleRuntimeException : java.lang.ClassNotFoundException: Cannot find implementation for com.yuha.xxxx.mapper.NticMapper
```

### 2) Custom Mapper 생성하기

* Ntic 엔티티의 다음 컬럼을 NticVo의 다음 컬럼으로 맵핑하고자 한다.
  * ntic01Id <-> seq
  * nticTitle <-> ask
  * nticCten <-> answer

#### 1. Ntic 엔티티 생성

```java
import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

import lombok.Data;

@Entity
@Table(name="ntic01tl")
@Data
public class Ntic01tl {
    @Id 
    @Column(name = "ntic01_id")		
    private String ntic01Id;
    
    @Column(name = "ntic_title")		
    private String nticTitle;

    @Column(name = "ntic_cten")		
    private String nticCten;
    
    @Column(name = "dsp_yn")		
    private String dspYn;
    
    @Column(name = "rgstr_dttm")		
    private Date rgstrDttm;
}

```

#### 2. NticVo 클래스 생성

```java
import io.swagger.annotations.ApiModelProperty;
import io.swagger.v3.oas.annotations.media.Schema;
import lombok.Data;

@Schema(description = "QnA 정보")
@Data
public class NticVo {

	public NticVo() {
    }
	
	@ApiModelProperty(value="순번")
    private Integer seq;

	@ApiModelProperty(value="질문")
    private String ask;

	@ApiModelProperty(value="답변")
    private String answer;

}
```

#### 3. 맵핑 해보기

* Entity를 리스트 형태로 받아 하나씩 NticVo로 맵핑하여 NticVo로 이루어진 리스트를 만들어보고자한다.

```java
// 1. 커스텀 Mapper 선언
NticMapper INSTANCE = Mappers.getMapper(NticMapper.class);
// 2. 엔티티 리스트에서 하나씩 꺼내서 맵핑하기
List<NticVo> FAQList = ntic01List.stream().map(ntic01 -> NticMapper.INSTANCE.entityToNticVo(ntic01)).collect(Collectors.toList());
```

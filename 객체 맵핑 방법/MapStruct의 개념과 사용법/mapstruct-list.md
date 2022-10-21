# MapStruct로 List 바인딩 하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-10-21 Fri**

**last modified : 2022-10-21 Fri**

## \[1] 구현 코드

#### List\<AA> aList 인스턴스를 MapStruct를 통해 List\<BB> bList 인스턴스로 바인딩해보잣

### (1) 리스트 생성

```java
import java.util.ArrayList;

// 서비스 호출 : AA 리스트 조회
List<AA> aList = AAService.getAList1(ParamVo);

// BB리스트 선언
List<BB> bList = new ArrayList<BB>();
```

### (2) mapper 커스터마이징 하기

```java
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.factory.Mappers;

@Mapper(componentModel = "spring")
public interface AbMapper {
	AbMapper INSTANCE = Mappers.getMapper(AbMapper.class);
	
	@Mapping(source = "bbId", target = "aaId")
	@Mapping(source = "bbName", target = "aaName")
	BB AAToBB(AA aa);
}
```

### (3) 리스트 바인딩

```java
import org.mapstruct.factory.Mappers;
import com.yuha.mapper.AbMapper;

AbMapper INSTANCE = Mappers.getMapper(AbMapper.class);

// 리스트 바인딩
List<BB> bList = aList.stream().map(AA -> INSTANCE.AAToBB(AA)).collect(Collectors.toList());

```


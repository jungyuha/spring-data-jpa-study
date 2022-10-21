# ModelMapper로 List바인딩 하기

**기록 ✍️**

**author : jung yuha**

**first registered : 2022-10-21 Fri**

**last modified : 2022-10-21 Fri**

## \[1] 구현 코드

#### List\<AA> aList 인스턴스를 ModelMapper를 통해 List\<BB> bList 인스턴스로 바인딩해보잣

### (1) 리스트 생성

```java
import java.util.ArrayList;

// 서비스 호출 : AA 리스트 조회
List<AA> aList = AAService.getAList1(ParamVo);

// BB리스트 선언
List<BB> bList = new ArrayList<BB>();
```

### (2) 리스트 바인딩

```java
// Some codeavav
import org.modelmapper.ModelMapper;
import org.modelmapper.TypeToken;

ModelMapper modelMapper = new ModelMapper();
modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STANDARD);

// 리스트 바인딩
bList = modelMapper.map(aList, new TypeToken<List<BB>>() {}.getType())

```


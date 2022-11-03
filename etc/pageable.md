# \[Pageable] 페이징 처리하기

<pre class="language-cpp"><code class="lang-cpp">// DB 테이블명 : test1 -> 엔티티명 : test1Entiity
// queryDSL을 사용해 조회한다.
<strong>QTestEntity testEntity = QTestEntity.test1;
</strong>BooleanBuilder builder = new BooleanBuilder();
    	
List&#x3C;test1Entiity> test1List =  new ArrayList&#x3C;test1Entiity>();

// 페이징 기준 : 등록일자 내림차순 + 10개씩
Pageable pageRequest = PageRequest.of(0,10,Sort.by(Sort.Direction.DESC,"rgstrDate"));

// 조회
Page&#x3C;test1Entiity> result = test1Repository.findAll(builder, pageRequest);

// 조회 결과
System.out.print("페이징된 건수(10건) : "+ImmutableList.copyOf(result));
System.out.print("페이지수 : "+result.getTotalPages());
System.out.print("전체 건수 : "+result.getTotalElements());</code></pre>

- [ ] 토비의 스프링 7장 ➕ 2025-12-14
- [ ] 토비의 스프링 8장 ➕ 2025-12-14 
- [ ] 토비의 스프링 9장 ➕ 2025-12-14 
- [ ] 자바 성능 튜닝 이야기 2장 ➕ 2025-12-14 
- [ ] 자바 성능 튜닝 이야기 3장 ➕ 2025-12-14 


- [x] @TestInstance annotation ➕ 2025-12-14 ✅ 2025-12-14
- [x] exclusive lock, shared lock, intension lock 을 실제로 postgresql 에서 어떤식으로 사용하는지 ➕ 2025-12-14 ✅ 2025-12-15
	- [ ] intension lock -> table-level lock 과 관련이 있음. 
	- [ ] PostgreSQL 의 6가지 table-level lock 
	- [ ] 4가지 row-level Lock
	- [ ] page level locks
	->이 3가지 level lock을 이해하기 위해 어떻게 postgreSQL 이 데이터를 다루는지 이해해야 함
	- [x] postgresql 에서의 inserting, deleting, and updating tuples ✅ 2025-12-19
- [x] PostgreSQL 의 기본적인 파일 구조 - Heap Table, page, page header, tuple, tuple header ➕ 2025-12-15 ✅ 2025-12-15
- [x] postgreSQL 이 ACID 중 A와 I  를 보장하기 위해 사용하는 테크닉 SI ✅ 2025-12-15

- [x] SI 의 기본 개념, SI 와 anomaly, SSI  ✅ 2025-12-15
- [x] txid ✅ 2025-12-15
- [x] What is @MockBean and its usage ➕ 2025-12-15 ✅ 2025-12-15
	- [x] MockBean 과 application Context ✅ 2025-12-15
		- [x] 오해 ✅ 2025-12-18
- [x] Controller 를 테스트 한다는 것은 뭘 테스트한다는 것인가 ? ➕ 2025-12-15 ✅ 2025-12-15
	- [x] Controller Test code 에서 뭐가 Mockito 가 관여하는 부분인가 ? ➕ 2025-12-15 ✅ 2025-12-15
	- [x] given 은 왜 쓰는가 ? ➕ 2025-12-15 ✅ 2025-12-15
	- [x] verify 는 왜 쓰는가 ? ➕ 2025-12-15 ✅ 2025-12-15
	      - jsonPath

- [x] Mockito.mock() vs @Mock vs @MockBean ➕ 2025-12-15 ✅ 2025-12-15
	- [x] Mock 객체의 동작 정의 (usage) ✅ 2025-12-15
	- [x] `when(mock.method()).thenReturn(value)` 과 `doReturn(value).when(mock).method()` 방식의 차이 ✅ 2025-12-15
		- [x] @Spy 객체 ✅ 2025-12-15
		- [x] **Void 메서드** (`doNothing`, `doThrow` 등) ✅ 2025-12-15
	- [x] Unstubbed Invocation ✅ 2025-12-15
	- [x] InjectMocks ✅ 2025-12-15
	- [x] Mockito 의 Mock 대신에 @MockBean 을 써야하는 상황? 이유? ✅ 2025-12-16
		- [x] Context Recreation ✅ 2025-12-16
			- [x] Spring 의 Context Caching 원리 ✅ 2025-12-16
			- [x] How to handle prevent Recreation ✅ 2025-12-16
- [x] MockMvc ✅ 2025-12-18
      @_WebMvcTest_ vs @_SpringBootTest_, @_SpringBootTest_ 와 함께 사용시 주의점
      what is @_**Autoconfigured**_
      test using TestRestTemplate and Mockbean
	- [x] refEq 관련 문제 ➕ 2025-12-18 ✅ 2025-12-18
	      Mockito 메서드가 실행이 되질 않는다 -> 무슨일이 벌어지는가 ?
		- [x] 같은 타입의 객체, 같은 값을 가진 필드여도 ✅ 2025-12-18
		      Mockito 의 메서드의 파라미터에 들어간 객체와
		      Controller 의 파라미터로 넘어온 객체가 다르면 무슨 문제가 생기는가 ?
		- [x] 이에 대한 해결책은 ? -> ArgumentCaptor ✅ 2025-12-18
		- [x] 근데 난 모든 필드를 비교해야하는데, equals 보다 비용이 더 나가는거 아닌가? ✅ 2025-12-18
		      -> `assertThat.usingRecursiveComparison`
		- [x] verify.usingRecursiveAssertion 의 순서 문제 ✅ 2025-12-18
		- [x] wrong usage of Custom Handler (signature is wrong with parameter) ✅ 2025-12-18
		- [x] `usingRecursiveAssertion` vs `usingRecursiveComparison` ✅ 2025-12-18

- [x] Exception 발생시 Controller 에서 어떻게 처리하게 할 것인가 ? ➕ 2025-12-16 ✅ 2025-12-16
      -> Exception 을 공부하는 이유 : Controller Test 때문
	- [x] @ExceptionHandler ✅ 2025-12-16
	- [x] @RestControllerAdvice ✅ 2025-12-16
	- [x] 커스텀 익셉션 핸들러에서 중복되는 코드는 어떻게 처리하는가 ? -> 예외처리 흐름에 대한 이해 ✅ 2025-12-17
		- [x] 기존에 존재하던 Exception 처리 흐름 ✅ 2025-12-17
		- [x] Custom Exception ✅ 2025-12-17
		- [x] Return Type 에 따라 달라지는 분기 ✅ 2025-12-17
		- [x] 그렇다면, handleExceptionInternal 에서 리플렉션으로 커스텀익셉션의 getProblemdetail 이 있는지 판단하면 되는건가? ✅ 2025-12-17
		- [x] ErrorResponse ✅ 2025-12-17
	- [x] ProblemDetail (RFC 9457, RFC 7807) ✅ 2025-12-16
		- [x] 내 Custom Exception 과 겹치는 거 아냐 ? -> Spring MVC Exception Handling Mechanism with  RestControllerAdvice ✅ 2025-12-16
		- [x] spring.mvc.problemdetails.enabled=true 적용시 Custom Exception 이 발생한다면, 또 기존 Exception 은 어떻게 처리되는지 ? ✅ 2025-12-16
- [ ] Unification of BiConsumer and BiFunction 
	- [x] Consumer, BiConsumer, BiFunction ✅ 2025-12-19
	- [x] void-compatible, value compatible ✅ 2025-12-19


- [x] PayManagementController 생성 및 테스트 ➕ 2025-12-14 ✅ 2025-12-18


- [ ] github action 설정 ➕ 2025-12-18 

- [ ] java 각 버전에 따른 language changes 정리 ➕ 2025-12-14 
- [ ] java 각 버전에 따른 features 정리 ➕ 2025-12-14 


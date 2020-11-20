# Junit5 를 시작하며



[Referenced From if(kakao) 2020](https://if.kakao.com/session/108)



### 동기

* Junit4 를 사용하다 보니, 이는 더 이상 Release 계획이 없음.
* Junit4 대신 Juni5 로 개발하고 있는 상태
* 최신 다양한 테스트 라이브러리는 Junit5 기반
* 많은 오픈소스들도 Junit5 기반 가이드 작성 중



### Why we choose junit5?

**기존 Junit4 문제점**

* IDE 강한 결합도
* @RunWith 부족한 확장성
  * 테스트 마다 @Runwith annotation 사용해야 됨
  * IDE, Extension, 3rd party 모두가 junit jar 에 의존.
* Big Jar
  * 하나의 클래스에 너무 많은 의존성을 가진 구조



**junit5 basic**

* Simple
* Extension

### 

### Difference Between Junit4 and Junit5



**junit4**

* public 을 써야됨

```java
@Test
public void testInPublic() {
    assertEquals("Public Test", "JUnit4");
}
```



**junit5**

* public 을 안써도 됨
* **Extension** = Runner + MethodRule + TestRule (in JUnit4)
* @RunWith 대신 **@ExtendWith 을 사용**

```java
@Test
void testInPackage() {
    assertEquals("Public Test", "JUnit5");
}
```

### Junit5 Components


* Vintage
* Jupiter
  * 왜 이름을 Jupiter 로?? -> 기존 Junit4 와 구분하기 위한 이름이 필요하고 목성이 5번째 행성이기 때문
* Platform
  * Platform Engine, Platform Launcher 를 통해 소스와 개발툴을 분리



### Junit 4 -> Junit 5

* @BeforeClass -> @BeforeAll
* @Before -> @BeforeEach
* @Test 
* @After -> @AfterEach
* @AfterClass -> @AfterAll
* @Category -> @Tag
* @Ignore -> @Disabled



### Junit5



**assertAll**

* Junit4 는 하나의 유닛이 실패하면 다른 유닛이 실행이 안됨
* 하나의 namespace 에 여러 개 assert 실행 가능
* 중간에 선언된 assert 문이 실패하여도, 선언된 모든 assert 문은 모두 실행 후 결과 반환

```java
@Test
void assertAllTest() {
    String name = "kakaoStory";
    assertAll("String Lower/Upper Case Test",
    	() -> assertEquals("kakaostory", name.toLowerCase()),
    	() -> assertEquals("KAKAOSTORY", name.toUpperCase()),              
    )
}
```



**assertThrows**

* Junit4 는 예외 메시지 검증은 별도 방법 필요
* Exception 을 쉽게 검증 가능
* assertDoesNotThrows 를 통해 예외 발생하지 않는 것도 검증 가능

```java
@Test
void assertThrowsTest() {
    Exception exception = assertThrows(
        ArithmeticException.class, () -> calculator.divide(1,0)
    );
    assertEquals(" / by zero ", exception.getMessage());
}
```



**assertTimeout**

* Junit4 에서는 annotation 으로 테스트 실행 시간 검증
* 테스트 실행시간에 대한 assert 기능 추가
* assertTimeoutPreemptively : 기대 시간을 초과할 시 테스트는 즉시 실패 및 종료

```java
@Test
void assertTimeoutTest() {
    // 5 seconds
    assertTimeout(ofSeconds(1), () -> { Thread.sleep(5000); });
    // 1 seconds
    assertTimeoutPreemptively(ofSeconds(1), () -> { Thread.sleep(5000); });    
}
```



**@DisplayName**

* 한글, 스페이스, 이모지, 특수문자 모두 가능
* Class, Method Level 선언 가능

```java
@DisplayName("DisplayName 추가됨")
class DisplayNameDemo {
    @Test
    @DisplayName("스페이스, ㅎㅎㅎ")
    void displayNameTest() {
        
    }
}
```



**@Nested**

* 계층 구조 테스트가 작성 안되었지만, 이를 해결함
* BDD 스타일 테스트도 가능

```java
@DisplayName("Calculator class")
public class CalculatorTest {
    private Calculator cal = new Calculator();
    @Nested
    @DisplayName("Plus Method 는")
    class PlusMethod {
        @Nested
        @DisplayName("더하는 숫자가 음수인 경우")
        class Handle_Minus_Number {
            private final int minusSum = -2;
            
            @Test
            @DisplayName("0과 음수를 더하면 음수가 반한됨")
            void return_sum_of_minus_number() {
                assertEquals(minusSum, cal.sum(minusSum, 0));
            }
        }
    }
}
```



**@ParameterizedTest**

* 여러 테스트를 매개 변수 형태로 간편히 사용 가능
* 최소 1개 이상 source annotation 필요
* Null, Empty, Value, Csv, Enum, Method 등 다양한 소스 형태 존재

```java
@ParameterizedTest
@EnumSource(value = City.class, names = { "SEOUL", "PARIS" })
void CityEnumTest(City city) {
    assertTrue(EnumSet.of(City,SEOUL, City.PARIS),contains(city));
}
```



**@TestFactory**

* 기존 테스트는 컴파일 시점으로 제한되지만, 테스트가 런타임 환경으로 생성 및 수행 가능
* 이를 통해 외부 자원 활용 및 랜덤 데이터 생성 활용 가능
* 타이틀 활용 시 가독성 높은 테스트 가능



```java
@TestFactory
Stream<DynamicNode> dynamicTests() {
    return Stream.of("ifkakao", "junit5", "kakaostory")
			.map(text -> dynamicTest( "Include Kakao", () -> assertTrue(text.contains("kakao"))));
}
```



### Spring with Junit5

**Junit4 & SpringTest**

* 하나의 @RunWith 만 사용, @Rule 을 통한 확장

```java
@RunWith(MockitoJunitRunner.class)
public class SpringRuleTest {
    @ClassRule
    public static final SpringClassRule classRule = new SpringClassRule();
    @Rule
    public SpringMethodRule methodRule = new SpringMethodRule();
}
```



**Junit5 & SpringTest**

* @Rule 대신 @ExtendWith(SpringExtension.class) 추가 하면 됨
* Spring Framework 5.0 이후 Junit5 가 통합됨
* Spring boot 2.2.0 이후 버전은 Junit5 가 기본


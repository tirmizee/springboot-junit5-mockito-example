# springboot-junit5-mockito-example

Spring boot 3 include junit 5 by default. 

### Dependencies

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

### Ex. 1 

```java
@SpringBootTest
class UserServiceTest {

    @Autowired
    UserService userService;

    @Test
    void testInformation() {

        // Given
        String expected = "user information";

        // When
        String actual = userService.information();

        // Then
        Assertions.assertEquals(expected, actual);
    }

}
```

### Ex. 2

```java
@SpringBootTest
class UserServiceTest {

    @Autowired
    UserService userService;

    @Test
    void testException() {

        // Given
        Class<NullPointerException> expected = NullPointerException.class;

        // When
        Executable executable = () -> userService.information(false);

        // Then
        Assertions.assertThrows(expected, executable);
    }

}
```

### Ex. 3 

```java
class LifecycleTest {

    @BeforeEach
    void setUp() {
        System.out.println("set up data");

    }

    @AfterEach
    void tearDown() {
        System.out.println("clear data");
    }

    @Test
    void testOne() {
        System.out.println("testOne");
    }

    @Test
    void testTwo() {
        System.out.println("testTwo");
    }

}
```

### Ex. 4 

```java
public class ParameterTest {

    @ParameterizedTest
    @ValueSource(strings = {"AAA", "BBB"})
    void withValueSource(String word) {

        // When
        boolean result = TextUtils.isWhitelist(word);

        // Then
        assertTrue(result);
    }

}
```


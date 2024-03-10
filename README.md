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

Ex. 5

```java
public interface UserRepository extends CrudRepository<UserEntity, Long> {
    Optional<UserEntity> findByUsername(String username);
}
```
```java
@Service
@RequiredArgsConstructor
public class UserService {

    private UserRepository userRepository;

    public UserEntity findByUsername(String username) {
        return userRepository.findByUsername(username)
                .orElseThrow(() -> new NoSuchElementException("No value present"));
    }

}
```
```java
@SpringBootTest
@ActiveProfiles("ut")
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    public void whenFindUserByEmail_thenReturnUser() {

        // Given
        UserEntity mockUser = new UserEntity();
        mockUser.setUserId(1L);
        mockUser.setUsername("tirmizee");
        when(userRepository.findByUsername(anyString())).thenReturn(Optional.of(mockUser));

        // When
        UserEntity found = userService.findByUsername("tirmizee");

        // Then
        assertEquals("tirmizee", found.getUsername());

    }


}
```



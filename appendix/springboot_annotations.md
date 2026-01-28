# Spring Boot Core Annotations Guide (v3.5.10)

이 문서는 Spring Boot 3.5.10 환경에서 웹 애플리케이션 개발 시 필수적으로 사용되는 핵심 어노테이션들을 정리한 가이드입니다.

---

## 1. 프로젝트 설정 및 메인 (Core)

### @SpringBootApplication
- **설명**: Spring Boot의 핵심 어노테이션입니다. `@Configuration`, `@EnableAutoConfiguration`, `@ComponentScan` 세 가지를 합친 역할을 합니다.
- **위치**: 주로 프로젝트의 메인 클래스에 위치합니다.

```java
@SpringBootApplication
public class MyApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApiApplication.class, args);
    }
}
```

## 2. 빈(Bean) 정의 및 의존성 주입 (DI)

**@Component / @Service / @Repository / @Controller**

- **설명**: 해당 클래스를 Spring 컨테이너가 관리하는 Bean으로 등록합니다.
    - ``@Service``: 비즈니스 로직 레이어
    - ``@Repository``: 데이터 엑세스 레이어 (Persistence)
    - ``@Controller``: Web MVC 컨트롤러
    - ``@RestController``: @Controller + @ResponseBody (JSON 반환용)

**@@Configuration & @Bean**

- **설명**: 자바 기반 설정 파일임을 명시하고, 메서드에서 반환되는 객체를 Bean으로 등록합니다.

```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

**@Autowired**
- **설명**: 필요한 의존성을 자동으로 주입합니다. (생성자, 필드, 메서드 주입 가능)
- Tip: Spring Boot 3.x에서는 생성자 주입을 가장 권장합니다.

<br>

## 3. Web MVC 및 REST API

**@RequestMapping & HTTP Method Annotations**

- **셜명**: 특정 URL 경로를 메서드에 매핑합니다.
- **종류**: ``@GetMapping``, ``@PostMapping``, ``@PutMapping``, ``@DeleteMapping``, ``@PatchMapping``

```java
@RestController
@RequestMapping("/api/v1/users")
public class UserController {

    @GetMapping("/{id}")
    public UserResponse getUser(@PathVariable Long id) {
        return userService.findById(id);
    }

    @PostMapping
    public ResponseEntity<Void> createUser(@RequestBody UserRequest request) {
        userService.save(request);
        return ResponseEntity.status(HttpStatus.CREATED).build();
    }
}
```

**파라미터 제어 어노테이션**

| 어노테이션 | 설명 | 예시 |
| :--- | :--- | :--- |
| @PathVariable | URL 경로에 표함된 변수 추출 | /users/{id} |
| @RequestParam | 쿼리 파라미터 추출 | /user?name=test |
| @RequestBody | HTTP 본문(JSON 등)을 객체로 변환 | POST 데이터 |
| @ResponseBody | 반환값을 HTTP 본문에 직접 기록 | JSON 응답 |

<br>

## 4. 설명 및 프로퍼티 (Properties)

- **@Value**
    - **설명**: application.properties나 application.yml에 설정된 값을 필드에 주입합니다.

- **@ConfigurationProperties**
    - **설명**: 계층적인 프로퍼티 설정을 자바 객체에 매핑합니다. (Type-safe)

```java
@ConfigurationProperties(prefix = "mail")
public class MailProperties {
    private String host;
    private int port;
}
```

<br>

## 5. 트랜잭션 및 예외처리

- **@Transaction**
    - **설명**: 메서드나 클래스를 트랜잭션 범위로 지정합니다. 오류 발생 시 자동 롤백을 수행합니다.

- **@@RestControllerAdvice & @ExceptionHandler**
    - **설명**: 애플리케이션 전역에서 발생하는 예외를 한 곳에서 처리합니다.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<String> handleNotFound(UserNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
    }
}
```

<br>

## 6. 테스트 (Testing)

- **@SpringBootTest**: 통합 테스트를 위해 애플리케이션 컨텍스트를 전체 로드합니다.
- **@WebMvcTest**: 컨트롤러 레이어만 테스트할 때 사용합니다.
- **@MockBean**: 테스트 시 가짜 객체(Mock)를 빈으로 등록합니다.
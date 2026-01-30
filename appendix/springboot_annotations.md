# Spring Boot 어노테이션 총량 및 분포

Spring 생태계가 워낙 거대하다 보니 정확한 "개수"를 한정 짓기 어렵지만, 일반적으로 다음과 같은 분포를 보입니다.

- **전체 어노테이션**: 약 500개 이상 (Spring Core, Web, Data, Security, Cloud 등 포함)
- **실무 빈출 어노테이션**: 약 80개
- **주요 50선**: 실제 코드의 90% 이상을 점유하는 핵심

---

<br>

## 1. 프로젝트 설정 및 메타 설정 (Config)
애플리케이션의 시작점과 외부 설정을 구성합니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@SpringBootApplication** | 앱 시작점 (3가지 설정 통합) | `@SpringBootApplication public class App {...}` |
| **@Configuration** | 빈 설정 클래스 명시 | `@Configuration public class AppConfig {...}` |
| **@Bean** | 수동 빈 등록 (메서드 반환 객체) | `@Bean public MyService myService() { ... }` |
| **@ComponentScan** | 빈 스캔 패키지 경로 지정 | `@ComponentScan(basePackages = "com.example")` |
| **@Import** | 다른 설정 클래스 로드 | `@Import({SecurityConfig.class, DbConfig.class})` |

<br>

## 2. 의존성 주입 및 빈 관리 (DI/IoC)
객체 간의 관계를 설정하고 생명주기를 관리합니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@Component** | 일반적인 스프링 관리 빈 등록 | `@Component public class Utility {...}` |
| **@Autowired** | 타입에 맞는 빈 자동 주입 | `@Autowired private UserService userService;` |
| **@Qualifier** | 동일 타입 빈 중 이름으로 식별 | `@Qualifier("mainService")` |
| **@Primary** | 동일 타입 빈 중 우선순위 부여 | `@Primary @Service public class DefaultService...` |
| **@Value** | 설정 파일(Properties/YML) 값 주입 | `@Value("${server.port}") private int port;` |
| **@PostConstruct** | 빈 초기화 직후 실행 메서드 | `@PostConstruct public void init() {...}` |
| **@PreDestroy** | 빈 소멸 직전 실행 메서드 | `@PreDestroy public void cleanup() {...}` |

<br>

## 3. Web MVC / REST API (Web)
HTTP 요청을 처리하고 응답을 반환하는 접점입니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@RestController** | @Controller + @ResponseBody | `@RestController public class ApiController {...}` |
| **@RequestMapping** | URL 패턴 매핑 (클래스/메서드) | `@RequestMapping("/api/v1")` |
| **@GetMapping** | HTTP GET 요청 매핑 | `@GetMapping("/{id}")` |
| **@PostMapping** | HTTP POST 요청 매핑 | `@PostMapping("/save")` |
| **@PutMapping** | HTTP PUT (수정) 요청 매핑 | `@PutMapping("/update")` |
| **@DeleteMapping** | HTTP DELETE (삭제) 요청 매핑 | `@DeleteMapping("/remove")` |
| **@PathVariable** | URL 경로 내 변수 추출 | `public User get(@PathVariable Long id)` |
| **@RequestParam** | 쿼리 파라미터(?key=val) 추출 | `public String search(@RequestParam String name)` |
| **@RequestBody** | HTTP 요청 바디(JSON)를 객체화 | `public void add(@RequestBody UserDto user)` |
| **@ResponseBody** | 반환 객체를 응답 바디로 변환 | `@ResponseBody public User getUser()` |
| **@ResponseStatus** | 응답 시 특정 HTTP 상태 코드 지정 | `@ResponseStatus(HttpStatus.CREATED)` |
| **@RequestHeader** | HTTP 요청 헤더 값 추출 | `public void log(@RequestHeader("UA") String ua)` |

<br>

## 4. 레이어드 아키텍처 (Layered Architecture)
역할에 따라 클래스의 성격을 규정합니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@Service** | 비즈니스 로직 처리 클래스 명시 | `@Service public class OrderService {...}` |
| **@Repository** | DB 접근 계층 (DAO) 명시 | `@Repository public interface UserRepository...` |
| **@Controller** | View(HTML) 반환 컨트롤러 명시 | `@Controller public class WebController {...}` |

<br>

## 5. 데이터베이스 및 트랜잭션 (JPA/DB)
데이터 영속성 관리와 데이터 정합성을 담당합니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@Transactional** | 트랜잭션 범위 및 롤백 관리 | `@Transactional public void transfer() {...}` |
| **@Entity** | DB 테이블과 매핑될 JPA 클래스 | `@Entity public class User {...}` |
| **@Id** | 엔티티의 기본키(PK) 지정 | `@Id @GeneratedValue private Long id;` |
| **@Column** | 필드와 DB 컬럼 상세 매핑 | `@Column(name = "user_name", nullable = false)` |
| **@Enumerated** | Enum 저장 방식 지정 | `@Enumerated(EnumType.STRING)` |
| **@Transient** | DB 매핑에서 제외할 필드 | `@Transient private String tempCode;` |

<br>

## 6. 검증 및 예외 처리 (Validation/Advice)
데이터 무결성 검사 및 전역 에러 처리를 수행합니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@Valid / @Validated** | 객체 제약 조건 검증 실행 | `public void save(@Valid @RequestBody Dto dto)` |
| **@NotNull** | null 허용 안 함 | `@NotNull private String email;` |
| **@Size** | 문자열/컬렉션 크기 제한 | `@Size(min = 2, max = 10)` |
| **@ExceptionHandler** | 특정 예외 처리 메서드 지정 | `@ExceptionHandler(Exception.class)` |
| **@ControllerAdvice** | 전역 예외 처리기 클래스 선언 | `@ControllerAdvice public class GlobalHandler...` |
| **@ResponseStatus** | 예외 발생 시 특정 상태 코드 반환 | `@ResponseStatus(HttpStatus.NOT_FOUND)` |

<br>

## 7. 테스트 (Testing)
코드의 품질을 검증하기 위한 도구입니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@SpringBootTest** | 전체 통합 테스트 (컨텍스트 로드) | `@SpringBootTest class MyTest {...}` |
| **@WebMvcTest** | 웹 레이어(Controller) 슬라이스 테스트 | `@WebMvcTest(UserApiController.class)` |
| **@DataJpaTest** | JPA 관련 빈만 테스트용 로드 | `@DataJpaTest class RepoTest {...}` |
| **@MockBean** | 가짜 객체(Mock)를 만들어 주입 | `@MockBean private RemoteService service;` |
| **@Test** | JUnit 5 테스트 메서드 선언 | `@Test void testMethod() {...}` |

<br>

## 8. 기타 유용한 기능 (Utility)
생산성 향상을 위한 비동기, 스케줄링 등 부가 기능입니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| **@ConfigurationProperties** | 외부 설정을 자바 객체로 바인딩 | `@ConfigurationProperties("app.auth")` |
| **@EnableScheduling** | 스케줄링 기능 활성화 | `@EnableScheduling @SpringBootApplication` |
| **@Scheduled** | 특정 주기마다 메서드 실행 | `@Scheduled(fixedRate = 5000)` |
| **@Async** | 비동기 메서드 실행 (별도 스레드) | `@Async public void sendEmail() {...}` |
| **@Lazy** | 사용 시점까지 빈 생성 지연 | `@Lazy @Autowired private MyBean bean;` |
| **@Profile** | 특정 환경(dev/prod)에서만 활성화 | `@Profile("prod") @Component...` |

<br>

## 9. 고급 설정 및 조건부 로드 (Conditional)
생산성 향상을 위한 비동기, 스케줄링 등 부가 기능입니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
|@ConditionalOnProperty | 특정 설정값(YML/Props) 존재 시에만 빈 등록 | ``@ConditionalOnProperty(name = "feature.enabled", havingValue = "true")`` |
| @ConditionalOnClass | 특정 클래스가 클래스패스에 있을 때만 작동 | ``@ConditionalOnClass(name = "com.mysql.cj.jdbc.Driver")`` |
| @DependsOn | 빈 생성의 우선순위(의존성 순서)를 강제 지정 | ``@DependsOn("databaseInitializer")`` |
| @ImportResource | 자바 설정 클래스에서 XML 설정 파일을 로드 | ``@ImportResource("classpath:legacy-context.xml")`` |

<br>

## 10. 성능 및 최적화 (Cache/Event)
반복적인 연산을 줄이거나 서비스 간의 결합도를 낮추어 시스템 성능을 향상시킵니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| @Cacheable | 메서드 결과를 캐시하여 동일 입력 시 재사용 | ``@Cacheable(value = "users", key = "#id")`` |
| @CacheEvict | 데이터 수정 시 저장된 캐시를 삭제(무효화) | ``@CacheEvict(value = "users", allEntries = true)`` |
| @EventListener | 애플리케이션 내부 이벤트를 구독/처리 | ``@EventListener public void handle(UserSignupEvent event)`` |

<br>

## 11. 보안 및 메시징 (Security/Messaging)
인증/인가 처리와 분산 시스템 간의 데이터 통신을 담당합니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| @PreAuthorize | 메서드 실행 전 권한(Role) 유무를 체크 | ``@PreAuthorize("hasRole('ADMIN')")`` |
| @AuthenticationPrincipal | 현재 로그인한 유저 세션 정보를 파라미터로 주입 | ``public void profile(@AuthenticationPrincipal User user)`` |
| @KafkaListener | 카프카(Kafka) 토픽 메시지를 수신하는 메서드 | ``@KafkaListener(topics = "order-topic")`` |

<br>

## 12. 데이터 정교화 및 기타 유틸리티
JSON 변환 제어 및 JPA의 성능을 미세 조정할 때 사용합니다.

| 어노테이션 | 설명 | 예시 코드 |
| :--- | :--- | :--- |
| @JsonIgnore | 객체를 JSON으로 변환할 때 해당 필드 제외 | ``@JsonIgnore private String password;`` |
| @JsonProperty | JSON의 키(Key) 이름과 필드명을 다르게 매핑 | ``@JsonProperty("user_id") private String userId;`` |
| @DynamicUpdate | 변경된 필드만 포함하여 SQL Update 실행 (JPA) | ``@DynamicUpdate @Entity public class User {...}`` |
| @Embedded | 여러 필드를 하나의 값 타입 객체로 그룹화 | ``@Embedded private Address address;`` |
| @Modifying | @Query를 통한 수정/삭제 쿼리임을 명시 | ``@Modifying @Query("update User u set u.name = :name")`` |
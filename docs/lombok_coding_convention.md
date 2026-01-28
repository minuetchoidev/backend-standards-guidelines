# Lombok 개발 표준 (Coding Convention)

본 문서는 Java / Spring 기반 프로젝트에서 **Lombok 사용을 표준화**하여
- 코드 가독성 향상
- 유지보수성 확보
- 무분별한 어노테이션 사용 방지
를 목적으로 한다.

---

## 1. 기본 원칙

1. **Lombok은 명확한 목적이 있을 때만 사용한다**
2. **엔티티(Entity)와 DTO의 사용 기준을 명확히 구분한다**
3. **@Data 사용은 원칙적으로 금지한다**
4. **생성자/Getter/Setter를 의도적으로 조합한다**
5. **IDE Lombok Plugin 필수 설치**

---

## 2. Lombok 사용 기준에 대한 공통 원칙 (WHY)

본 개발표준에서 **사용 기준 / 사용 제한 / 사용 금지**를 명시한 이유는 다음과 같다.

### 2.1 Lombok 사용의 핵심 철학

1. **Lombok은 생산성 도구이지 설계 도구가 아니다**
   - 클래스의 책임과 객체의 상태 변경 흐름은 코드 레벨에서 명확해야 한다.

2. **자동 생성 메서드는 의도를 숨길 수 있다**
   - Getter/Setter, equals, toString 등이 무분별하게 생성되면
     객체의 변경 가능성(Mutability)과 책임 범위가 불분명해진다.

3. **프레임워크(JPA/Spring)와 충돌 가능성**
   - JPA 프록시, 양방향 연관관계, Lazy Loading 환경에서
     Lombok 자동 메서드는 심각한 성능/버그 이슈를 유발할 수 있다.

4. **리뷰 가독성 및 유지보수성 저하 방지**
   - 코드 리뷰 시 "보이지 않는 코드"가 많을수록
     사이드 이펙트 파악이 어려워진다.

---

### 2.2 주요 Annotation별 사용 기준의 이유 (WHY)

#### @Getter는 허용, @Setter는 제한하는 이유
- Getter는 객체 상태를 **노출만** 함
- Setter는 객체 상태를 **외부에서 변경 가능**하게 만듦
- Entity에 Setter가 존재하면
  - 도메인 규칙을 우회한 값 변경 가능
  - 트랜잭션 범위 밖 변경 위험

➡️ **그래서 Entity에는 Setter를 제한한다**

---

#### @Data를 금지하는 이유

@Data는 아래를 모두 포함한다:
- @Getter
- @Setter
- @ToString
- @EqualsAndHashCode
- @RequiredArgsConstructor

문제점:
1. Entity에 적용 시
   - 모든 필드 Setter 자동 생성 → 무결성 붕괴
2. equals/hashCode 자동 생성
   - 연관관계 필드 포함 시 무한 루프
3. toString 자동 생성
   - Lazy Loading 트리거 가능

➡️ **의도를 통제할 수 없으므로 전면 금지**

---

#### @Builder를 Entity에 제한하는 이유

문제점:
- 모든 필드를 열어둔 객체 생성 구조
- 생성 시점에 도메인 검증 로직 누락 가능
- JPA Entity는 기본 생성자 + 명시적 행위 메서드가 핵심

➡️ **DTO에는 허용, Entity에는 지양**

---

#### @NoArgsConstructor(access = PROTECTED)를 강제하는 이유

- JPA는 리플렉션 기반 객체 생성을 사용
- public 생성자는 잘못된 객체 생성을 허용

➡️ **JPA 요구사항 + 도메인 보호를 동시에 만족**

---

#### @SneakyThrows를 금지하는 이유

문제점:
- 체크 예외가 코드 상에서 사라짐
- 호출자 레벨에서 예외 인지 불가
- 장애 분석 시 스택 추적 난이도 증가

➡️ **명시적 예외 처리가 품질을 높인다**

---

## 3. Lombok Annotation 전체 목록 및 개발 표준

> ⚠️ 예시는 이해를 돕기 위한 간단한 코드이며, 실제 프로젝트 상황에 맞게 조정 가능

---

### 2.1 @Getter

#### ✅ 사용 기준
- DTO, Entity, VO 등 읽기 전용 필드에 사용 가능

```java
@Getter
public class UserDto {
    private String userId;
    private String userName;
}
```

---

### 2.2 @Setter

#### ⚠️ 사용 제한
- DTO 외 사용 지양
- Entity에는 **부분 적용(@Setter(AccessLevel.NONE)) 권장**

```java
@Setter
public class UserRequestDto {
    private String userName;
}
```

---

### 2.3 @ToString

#### ⚠️ 주의사항
- Entity에서 사용 시 **연관관계 필드 제외 필수**

```java
@ToString(exclude = "password")
public class User {
    private String id;
    private String password;
}
```

---

### 2.4 @EqualsAndHashCode

#### 사용 기준
- VO(Value Object)에만 사용 권장

```java
@EqualsAndHashCode(of = "id")
public class Product {
    private Long id;
    private String name;
}
```

---

### 2.5 @NoArgsConstructor

#### 사용 기준
- JPA Entity에서 **protected 접근제어자 필수**

```java
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class User {
    @Id
    private Long id;
}
```

---

### 2.6 @AllArgsConstructor

#### 사용 기준
- 테스트 코드 또는 단순 DTO에서만 사용

```java
@AllArgsConstructor
public class SimpleDto {
    private String name;
    private int age;
}
```

---

### 2.7 @RequiredArgsConstructor

#### ✅ 권장
- final 필드 또는 @NonNull 필드 기반 생성자
- Spring 의존성 주입 시 최우선 사용

```java
@RequiredArgsConstructor
@Service
public class UserService {
    private final UserRepository userRepository;
}
```

---

### 2.8 @Builder

#### 사용 기준
- DTO 생성에 적극 권장
- Entity에는 사용 지양

```java
@Builder
@Getter
public class UserResponseDto {
    private String id;
    private String name;
}
```

---

### 2.9 @Value

#### 사용 기준
- 불변 객체(Immutable VO)에만 사용

```java
@Value
public class Money {
    int amount;
}
```

---

### 2.10 @Data ❌ (사용 금지)

#### ❌ 금지 사유
- @Getter, @Setter, @ToString, @EqualsAndHashCode, @RequiredArgsConstructor 포함
- 의도하지 않은 메서드 자동 생성 위험

```java
// ❌ 사용 금지
@Data
public class User {
    private String id;
}
```

---

### 2.11 @Slf4j

#### ✅ 사용 기준
- 모든 로깅 클래스에서 표준 로거로 사용

```java
@Slf4j
public class UserService {
    public void process() {
        log.info("User process start");
    }
}
```

---

### 2.12 @Log / @Log4j / @Log4j2 / @CommonsLog

#### ❌ 사용 금지
- **@Slf4j 단일 표준 사용**

---

### 2.13 @NonNull

#### 사용 기준
- 생성자 파라미터, 메서드 파라미터에 사용

```java
public void save(@NonNull String name) {
    // null 체크 자동 생성
}
```

---

### 2.14 @Cleanup

#### 사용 제한
- try-with-resources 대체 가능할 경우 사용 지양

```java
@Cleanup InputStream in = new FileInputStream("test.txt");
```

---

### 2.15 @SneakyThrows

#### ❌ 사용 금지
- 예외 흐름 추적 어려움

```java
// ❌ 사용 금지
@SneakyThrows
public void run() {
    throw new Exception();
}
```

---

### 2.16 @Synchronized

#### 사용 제한
- Java synchronized 키워드 우선 사용

```java
@Synchronized
public void syncMethod() {}
```

---

### 2.17 @UtilityClass

#### 사용 기준
- 순수 유틸리티 클래스에만 사용

```java
@UtilityClass
public class DateUtils {
    public String today() {
        return "2026-01-01";
    }
}
```

---

### 2.18 @With

#### 사용 기준
- 불변 객체 복사 시 사용

```java
@Value
public class User {
    String name;
    @With int age;
}
```

---

## 3. 계층별 Lombok 사용 가이드

| 계층 | 허용 Annotation |
|---|---|
| Entity | @Getter, @NoArgsConstructor, @ToString(exclude) |
| DTO | @Getter, @Setter, @Builder, @AllArgsConstructor |
| VO | @Value, @EqualsAndHashCode |
| Service | @RequiredArgsConstructor, @Slf4j |

---

## 4. 결론

- Lombok은 **코드 단축 도구이지 설계 도구가 아니다**
- 명시적인 코드가 필요한 경우 Lombok 사용을 피한다
- 본 표준은 팀 합의에 따라 지속적으로 개선한다

---

📌 *Last Updated: 2026-01-28*


# 백엔드 기술 표준 가이드

1. ``프로젝트 개요 (Overview)``

    - 표준화 목적: 코드 일관성 유지, 유지보수 효율화, 팀 협업 프로세스 정립

    - [환경 구성 (Tech Stack)](./docs/stack.md): Java 21, Spring Boot 3.5.10, Gradle, PostgreSQL, HikariCP 등 명시

    - 의존성 관리: io.spring.dependency-management 활용 및 라이브러리 추가 정책

2. ``프로젝트 구조 (Architecture)``

    - [아키텍처 모델](./docs/layed_architecture.md): 도메인 중심 레이어드 아키텍처 (Presentation > Application > Domain > Infrastructure)

    - 패키지 구조 표준: 기능(Domain) 기반 패키징 전략 (예: kr.co.sc301.domain)

    - 데이터 객체(Data Object) 정책:

    - Entity: DB 테이블 매핑 (비즈니스 로직 포함)

    - DTO: 레이어 간 데이터 전송 및 API 입출력 (필수 분리)

    - VO: 값 자체를 나타내는 불변 객체

    - Mapping: Entity-DTO 변환 방식 (MapStruct 사용 여부 등)

3. ``코딩 컨벤션 (Coding Convention)``

    - 코드 스타일: Google Java Style Guide 준수 및 IDE(IntelliJ) Formatter 공유

    - 명명 규칙 (Naming Rule):

        - Java: 클래스(Pascal), 메서드/변수(Camel), 상수(Upper Snake)

        - DB: 테이블/컬럼(Snake Case)

        - Lombok 가이드: @RequiredArgsConstructor 필수 사용, @Data 및 @AllArgsConstructor 사용 금지

        - 주석(Comment) 표준: 주요 비즈니스 로직에 대한 Javadoc 작성 및 API 설명 주석

4. ``공통 인프라 및 핵심 기능 (Common & Core)``

    - 공통 응답 (Common Response): ResultVO<T> 포맷 정의 (Status, Message, Data)

    - 예외 처리 (Exception Handling):

    - BusinessException (Unchecked) 정의 및 ErrorCode Enum 관리

    - @RestControllerAdvice 기반의 전역 에러 핸들링

    - 유효성 검사 (Validation): Bean Validation(@Valid) 사용 및 필드별 에러 메시지 처리 표준

    - 로그 관리 (Logging): Logback 기반 로그 레벨 운영 및 MDC를 활용한 Trace ID 추적

5. ``데이터 액세스 표준 (Data Access)``

    - JPA 표준: 연관관계 매핑(Lazy Loading 원칙), N+1 방지(Fetch Join), Querydsl 사용 기준

    - MyBatis 표준: 복잡한 통계 쿼리 사용 기준 및 XML/Mapper 인터페이스 위치 규정

    - 트랜잭션 관리: @Transactional 사용 위치 (Service Layer), readOnly = true 활용 권장

6. ``보안 표준 (Security)``

    - 인증 및 인가: Spring Security + JWT (Access/Refresh Token 전략 및 Redis 저장)

    - 보안 전용 객체: UserDetails 커스텀 및 SecurityContextHolder 접근 유틸리티

    - 개인정보 보호: 민감 정보 암호화(AES-256), 단방향 해시(BCrypt) 처리

    - 취약점 방어: CORS, XSS Filter, SQL Injection 방지 설정

7. ``테스트 표준 (Testing)``

    - 테스트 전략: JUnit5 & Mockito 기반 단위 테스트 작성 가이드

    - 통합 테스트: @SpringBootTest 및 Testcontainers (PostgreSQL) 활용 범위

    - 검증 도구: AssertJ를 이용한 가독성 높은 단언문 사용

    - 코드 품질: JaCoCo 기준 설정 (예: Line Coverage 80% 이상 유지)

8. ``환경 설정 및 API 문서화 (Configuration & Docs)``

    - 환경 설정 관리: application.yml 프로필(Local, Dev, Prod) 분리 및 보안 설정(jasypt)

    - API 명세: Swagger (SpringDoc) UI 설정 및 Rest Docs 연동 가이드

    - Build & Deploy:

    - Git: Git-Flow (feature -> develop -> main) 전략

    - CI/CD: GitHub Actions/Jenkins 파이프라인 및 Docker 이미지 빌드 공정

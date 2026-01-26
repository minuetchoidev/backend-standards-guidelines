# 프로젝트 기술 표준 가이드

1. 프로젝트 개요 (Overview)

표준화 목적: 일관된 코드 품질 유지 및 개발 생산성 향상

[환경 구성 (Stack): Java 버전, Spring Boot 버전, Build Tool(Gradle/Maven), RDBMS 등 명시](./docs/stack.md)

2. 프로젝트 구조 (Architecture)

레이어드 아키텍처 (Layered Architecture): Presentation - Service - Persistence 레이어의 역할 정의

패키지 구조: 기능별(Domain) 또는 계층별(Layer) 패키지 설계 표준

객체 명명 및 사용 표준: DTO, Entity, VO의 구분 및 변환(Mapping) 정책

3. 코딩 컨벤션 (Coding Convention)

코드 스타일: Google Java Style Guide 또는 네이버 핵데이 컨벤션 채택

Naming Rule: 클래스, 메서드, 변수, DB 테이블 및 컬럼 명명 규칙

Lombok 사용 규정: 생성자 주입(@RequiredArgsConstructor) 및 @Data 사용 금지 등

4. 공통 인프라 (Common Infrastructure)

공통 응답(Common Response): API 성공/실패 시의 공통 JSON 포맷

예외 처리(Exception Handling): CustomException 정의 및 @RestControllerAdvice를 통한 전역 처리

로그 관리(Logging): Logback 설정을 통한 로그 레벨(Trace, Debug, Info, Warn, Error) 및 포맷 표준화

5. 데이터 액세스 표준 (Data Access)

ORM(JPA) 표준: 영속성 컨텍스트 관리, N+1 문제 방지 전략, Querydsl 사용 기준

트랜잭션 관리: @Transactional 선언적 트랜잭션 사용 위치 및 전파 속성 정의

6. 보안 표준 (Security)

인증 및 인가: Spring Security 기반 JWT 또는 OAuth2 연동 방식

데이터 암호화: 개인정보 등 민감 정보의 양방향/단방향 암호화 정책

웹 보안: CORS 설정, CSRF 방지, SQL Injection/XSS 방어

7. 테스트 표준 (Testing)

테스트 전략: 단위 테스트(JUnit5, Mockito)와 통합 테스트의 범위 설정

코드 커버리지: JaCoCo 등을 활용한 최소 커버리지 기준 설정

8. API 문서화 및 배포 (Documentation & CI/CD)

API 명세: Swagger(SpringDoc) 또는 Spring Rest Docs 적용 가이드

빌드 및 배포: Git Flow(Branch 전략)와 CI/CD 파이프라인(Jenkins, GitHub Actions) 연동 과정


---

1. [개발 환경 및 기술 스택 (Tech Stack)](./docs/1.개발_환경_및_기술_스택(Tech_Stack).md)
2. 프로젝트 구조 및 패키징 (Project Structure)
3. [Coding Convention & lombok](./docs/3.Coding_Convention_&_lombok.md)
4. [Persistence Layer 표준 (Hybrid: JPA + MyBatis)](./docs/4.PersisenceLayer표준.md)
5. [API 설계 및 메시지 표준 (API Design)](5.API_설계_및_메시지_표준.md)
6. Exception Handling (예외처리)
7. 보안 및 인증 (Security)
8. Logging & Monitoring
9. [Test & Quality Assurance](./docs/9.Test_&_Quality_Assurance.md)
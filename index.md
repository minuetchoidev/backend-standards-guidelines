# 백엔드 기술 표준 가이드

1. 프로젝트 개요 (Overview)

    - **표준화 목적** : 코드 품질 상향 평준화, 기술 부채 최소화, 신규 인원 온보딩(On-board) 가속화

    - **적용 법위**: 프로젝트 내 모든 백엔드 모듈 및 API 서비스

2. [기술 스택 및 환경 구성 (Tech Stack)](./docs/stack.md)

    - Language: Java 21

    - Framework: Spring Boot 3.5.10

    - Database: PostgreSQL, Redis (Cache/Session)

    - Connection Pool: HikariCP

    - Build Tool: Gradle

2. ``프로젝트 구조 (Architecture)``

    - [아키텍처 모델](./docs/layed_architecture.md): 도메인 중심 레이어드 아키텍처 (Presentation > Application > Domain > Infrastructure)

    - 패키지 구조 표준: 기능(Domain) 기반 패키징 전략 (예: kr.co.sc301.domain)

    - 데이터 객체(Data Object) 정책

    - Entity: DB 테이블 매핑

    - DTO: 레이어 간 데이터 전송 및 API 입출력

    - VO: 값 자체를 나타내는 불변 객체

    - Mapping: Entity-DTO 변환 방식 (MapStruct 사용 여부 등)

3. ``공통 인프라 및 핵심 기능 (Common & Core)``

    - 공통 응답 (Common Response): ResultVO<T>

    - 에러 코드 관리: ResponseCode Enum

    - 유효성 검사 (Validation): @Valid

    - 비즈니스 예외 정의: BusinessException

    - 로그 관리 (Logging)

4. ``데이터 액세스 표준 (Data Access)``

    - JPA 표준: 연관관계 매핑(Lazy Loading 원칙), N+1 방지(Fetch Join), Querydsl 사용 기준

    - MyBatis 표준: 복잡한 통계 쿼리 사용 기준 및 XML/Mapper 인터페이스 위치 규정

    - 트랜잭션 관리: @Transactional 사용 위치 (Service Layer), readOnly = true 활용 권장

5. ``보안 표준 (Security)``

    - 인증 및 인가: Spring Security + JWT (Access/Refresh Token 전략 및 Redis 저장)

    - 보안 전용 객체: UserDetails 커스텀 및 SecurityContextHolder 접근 유틸리티

    - 개인정보 보호: 민감 정보 암호화(AES-256), 단방향 해시(BCrypt) 처리

    - 취약점 방어: CORS, XSS Filter, SQL Injection 방지 설정

6. ``테스트 표준 (Testing)``

    - 테스트 전략: JUnit5 & Mockito 기반 단위 테스트 작성 가이드

    - 통합 테스트: @SpringBootTest 및 Testcontainers (PostgreSQL) 활용 범위

    - 검증 도구: AssertJ를 이용한 가독성 높은 단언문 사용

    - 코드 품질: SonarQube 활용

7. ``환경 설정 및 API 문서화 (Configuration & Docs)``

    - 환경 설정 관리: application.yml 프로필(Local, Dev, Prod) 분리 및 보안 설정(jasypt)

    - API 명세: Swagger (SpringDoc) UI 설정

    - Build & Deploy:

    - Git: Git-Flow (feature -> develop -> main) 전략

    - CI/CD: GitHub Actions/Jenkins 파이프라인 및 Docker 이미지 빌드 공정

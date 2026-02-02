# □ 백엔드 표준 가이드 구현 로그맵

<br>

## 1. Phase1: 기반 인프라 및 핵심 구조 수립 (1주)

- **Week 1 (Step 2 & 3 일부)**
    - **프로젝트 환경 설정**: Java 21, Gradle, Spring Boot 환경 구성
    - **아키텍처 레이어링**: Presentation - Application - Domain - Infrastructure 패키지 구조 생성 및 의존성 규칙 설정
    - **데이터 객체 정책**: BaseEntity, BaseTimeEntity 정의 및 MapStruct/Lombok 설정
    - **공통 응답 체계**: ResultVO<T>, ResponseCode Enum, GlobalExceptionHandler 구현 및 Validation 통합 처리

<br>

## 2. Phase 2: 데이터 액세스 및 보안 기반 (3주)

데이터를 다루는 방식과 서비스 접근 권한을 정의합니다.

- **Week 2 (Step 4 & 5 일부)**

    - **DB 설정**: PostgreSQL 연결, HikariCP 최적화, JPA 및 Querydsl 설정 및 Jasypt를 이용한 민감 설정 암호화.
    - **보안 아키텍처**: KeyCloak 연동, Spring Security 필터 체인 구성, Redis 기반 토큰 관리 로직 구현.
    - **암호화 유틸**: 개인정보 암호화(AES-256) 및 보안 유틸리티 클래스 작성.

<br>

## 3. Phase 3: 테스트 및 품질 관리 표준 (1주)

개발자들이 작성한 코드를 검증할 시스템을 구축합니다.

- **Week 3 (Step 6)**
    - **테스트 환경**: JUnit5, Mockito 설정 및 Testcontainers를 이용한 DB 통합 테스트 환경 구축.
    - **공통 테스트 베이스**: AbstractTest 등 반복되는 설정을 줄이기 위한 추상 클래스 지원.
    - **코드 품질**: SonarQube 로컬/서버 연동 및 필수 체크리스트 정의.

<br>

## 4. Phase 4: 테스트 및 품질 관리 표준 (1주)

- **Week 4 (Step 7)**

    - **환경 설정**: application-dev/prod.yml 분리
    - **문서화**: SpringDoc(Swagger) 설정 및 API 문서 자동화 가이드 작성.
    - **CI/CD**: GitHub Actions/Jenkins 파이프라인 스크립트 작성 (Docker 이미지 빌드 및 배포 테스트).
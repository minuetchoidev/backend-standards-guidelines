# 레이어드 아키텍처 상세 역할 및 흐름

1. Presentation Layer (controll, dto)

    - Controller: 외부 API 요청을 수신하고 유효성 검사 (@Valid)를 수행합니다.
    - DTO: API 명세에 맞는 데이터만 노출하기 위해 사용합니다. 보안과 유연성을 위해 Entity를 직접 외부에 노출하지 않는 것이 원칙입니다.

2. Business/Service Layer (service, impl, vo)

    - Service: 도메인 로직을 조합하여 실제 비즈니스 요구사항을 해결합니다.
    - impl: 구체적인 구현을 분리하여 유지보수성을 높입니다. (최근에는 단순한 프로젝트의 경우 인터페이스 없이 Service 클래스만 두기도 합니다.)
    - VO: 비즈니스 도메인 내에서 '주소', '금액' 처럼 속성 자체가 값인 객체를 정의합니다.

3. Persistence/Infrastructure Layer (repository, entity)

    - Entity: 데이터베이스의 테이블과 1:1로 매핑되는 가장 중요한 객체입니다.
    - Repository: JPA 등을 활용해 DB 쿼리를 실행하는 인터페이스입니다.

# 레이어드 아키텍처 주석 가이드

```bash
domain
  └── user
      ├── controller   // [Presentation Layer] 클라이언트의 요청(HTTP)을 받고 응답을 반환하는 접점
      ├── dto          // [Data Transfer Object] 계층 간 데이터 교환을 위한 객체 (주로 가공된 데이터)
      ├── entity       // [Domain Model Layer] 실제 DB 테이블과 매핑되는 핵심 비즈니스 모델
      ├── mapper       // [Mapping Utility] Entity <-> DTO 변환 로직을 담당 (예: MapStruct, ModelMapper)
      ├── repository   // [Infrastructure Layer] DB에 접근하여 데이터의 CRUD를 수행 (Spring Data JPA 등)
      ├── service      // [Service Layer - Interface] 비즈니스 로직의 추상화 및 트랜잭션 경계 정의
      │    └── impl    // [Service Layer - Implement] 서비스 인터페이스의 실제 구현체
      └── vo           // [Value Object] 값 자체로 의미를 가지는 불변 객체 (Entity와는 식별자 유무로 구분)
```


> 전체 패키지 정보

```bash
  
\---src
    +---main
    |   +---java
    |   |   \---kr
    |   |       \---co
    |   |           \---sc301
    |   |               |   Medis2Application.java
    |   |               |   
    |   |               +---common
    |   |               |   +---annotation
    |   |               |   |       DataType.java
    |   |               |   |       ExcelColumn.java
    |   |               |   |       Mask.java
    |   |               |   |       
    |   |               |   +---config
    |   |               |   |       JwtConfig.java
    |   |               |   |       SecurityConfig.java
    |   |               |   |       SwaggerConfig.java
    |   |               |   |       WebConfig.java
    |   |               |   |       
    |   |               |   +---controller
    |   |               |   |       BaseController.java
    |   |               |   |       
    |   |               |   +---dto
    |   |               |   |       BaseDTO.java
    |   |               |   |       
    |   |               |   +---enums
    |   |               |   |       MaskingType.java
    |   |               |   |       ResponseCode.java
    |   |               |   |       
    |   |               |   +---exception
    |   |               |   |       BusinessException.java
    |   |               |   |       GlobalExceptionHandler.java
    |   |               |   |       
    |   |               |   +---service
    |   |               |   |       BaseService.java
    |   |               |   |       
    |   |               |   +---util
    |   |               |   |       ExcelDownload.java
    |   |               |   |       JwtUtil.java
    |   |               |   |       MaskingUtil.java
    |   |               |   |       
    |   |               |   \---vo
    |   |               |           BaseVO.java
    |   |               |           ResultData.java
    |   |               |           ResultVO.java
    |   |               |           
    |   |               +---domain
    |   |               |   \---user
    |   |               |       +---controller
    |   |               |       +---dto
    |   |               |       +---entity
    |   |               |       +---mapper
    |   |               |       +---repository
    |   |               |       +---service
    |   |               |       |   \---impl
    |   |               |       \---vo
    |   |               \---sample
    |   |                       ExceptionController.java
    |   |                       FailResultController.java
    |   |                       HelloController.java
    |   |                       SuccessResultController.java
    |   |                       
    |   \---resources
    |       |   application.yml
    |       |   banner.txt
    |       |   messages.properties
    |       |   messages_en.properties
    |       |   messages_ko.properties
    |       |   
    |       +---mapper
    |       |   +---common
    |       |   |       CommonSQL.xml
    |       |   |       
    |       |   \---domain
    |       |       \---user
    |       |               User.xml
    |       |               
    |       +---static
    |       \---templates
    \---test
        \---java
            \---kr
                \---co
                    \---sc301
                        |   Medis2ApplicationTests.java
                        |   
                        \---common
                            \---util
                                    MaskingTypeTest.java
                                    

```
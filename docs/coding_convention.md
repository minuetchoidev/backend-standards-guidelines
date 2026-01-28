# Coding Convention & Lombok

코드의 가독성을 높이고, 현대적인 Java 문법을 통해 실수를 줄이며 생산성을 극대화하는데 목적이 있습니다.

## Google Java Style Guilde 준수

우리 프로젝트는 코드의 일관성을 위해 [Google Java Style Guide]를 기본 표준으로 채택합니다.

- 도입이유: 개인별 코딩 스타일 차이로 인한 코드 리뷰의 피로도를 줄이고, 전 세계적으로 검증된 가독성 표준을 따르기 위함이빈다.

- 핵심 준수 사항:
    - 들여쓰기는 2 space 를 사용합니다.
    - 클래스 멤버는 일정한 순서(필드 → 생성자 → 메서드)로 배치합니다.
    - import 시 와일드카드(*) 사용을 금지합니다.
- 자동화: IDE(IntellJ 등)에 Google Style 포맷터를 적용하여 저장 시 자동 정렬되도록 설정합니다.

### 구글 코드 스타일 보기

- [구글 코드 스타일 한글보기](../appendix/google_code_style.md)
- [구글 코드 스타일 영문보기](https://google.github.io/styleguide/javaguide.html)

### Spring Boot 프로젝트에 구글 스타일 적용 방법

Spring Boot (Java) 개발자라면 IDE와 빌드 도구에 설정해서 자동으로 관리하는 것이 가장 편합니다.

- IntelliJ IDEA에서 설정하기

    1. 설정 파일 다운로드: [Google Style XML](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml)파일을 받습니다.
    2. 적용: Setting > Editor > Code Style > Java 에서 톱니바퀴 아이콘 클릭 후 Import Scheme를 선택해 다운로드한 파일을 불러옵니다.

- CheckStyle 활용하기

프로젝트 빌드 시점에 컨벤션을 강제하고 싶다면 Checkstyle 플러그인을 사용합니다.
    - Gradle: checkstyle 플러그인 추가 및 google.checks.xml 설정
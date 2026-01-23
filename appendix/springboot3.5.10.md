# Spring Boot 3.5.10

1. 현재 가장 안정적인 추천 버전
- 추천 버전: Spring Boot 3.5.10
- 이유: 3.4 버전의 유지보수가 종료되면서, 현재 보안 패치와 버그 수정이 가장 활발하게 반영되고 있는 안정화된 최신 버전이다.
- JWT 호환성: 3.4.x에서 쓰던 설정 코드를 그대로 사용할 수 있으며, 내부 라이브러리(Spring Security 6.x)의 보안 취약점이 모두 해결된 상태입니다.

2. 프로젝트 설정 시 가이드

만약 프로젝트를 새로 만드신다면, build.gradle 또는 pom.xml 에 다음과 같이 설정하는 것이 가장 안정적입니다.

- Gradle:
```gradle
plugins {
    id 'org.springframework.boot' version '3.5.10'
    id 'io.spring.dependency-management' version '1.1.7'
}
```

- Maven:
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.5.10</version>
</parent>
```
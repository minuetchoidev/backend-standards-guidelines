# 백엔드 기술 자료

## 백엔드 정의

1. 한 문장 정의: "보이지 않는 곳에서 데이터를 처리하는 엔진"

2. 백엔드의 핵심 역할 3가지
    - 데이터 관리: 사용자의 정보, 상품 목록, 결제 내역 등을 안전하게 저장하고 관리합니다.
    - 비즈니스 로직: 서비스의 규칙을 코드를 구현합니다.
    - 인증 및 보안: 로그인한 사용자가 본인이 맞는지, 접근 권한이 있는지 확인하고 데이터를 보호합니다. (KeyCloak, Spring Security 등)


## 데이터를 처리하는 엔진

1. [RDB (Postgre, Mysql)](./index.md)
2. NoSQL (Amazon DynamoDB, MongoDB) 
3. In-Memory, Redis (보조)
4. MQ Asynchronous (Kafka/RabbitMQ)
5. 3rd Party, Open Source, Search Engine (Elasticsearch, OpenSearch)
6. Object Storage (AWS S3)
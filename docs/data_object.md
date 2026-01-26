1. JPA 관점의 데이터 객체 정책

    JPA는 객체 지향적인 설계를 지향하므로, 객체가 스스로의 상태를 관리하는 것이 핵심입니다.

    - Entity 중심성: Entity가 단순히 데이터를 담는 통이 아니라, 비즈니스 로직을 가진 '생명체' 역할을 합니다.

    - 지연 로딩(Lazy Loading): Entity 간의 연관관계를 설정해두고 필요한 시점에 데이터를 가져오므로, DTO로 변환할 때 이 시점을 잘 제어해야 합니다.

    - 변경 감지(Dirty Checking): update 쿼리를 직접 날리지 않고, Entity의 상태를 변경하면 트랜잭션 종료 시 DB에 반영됩니다.

> JPA 예시 구조

```java
// JPA Entity: 비즈니스 로직 포함
@Entity
public class Member {
    @Id @GeneratedValue
    private Long id;
    private String name;
    private int point;

    // 비즈니스 로직: 포인트 적립은 엔티티 내부에서 수행
    public void addPoint(int amount) {
        this.point += amount;
    }
}
```

2. MyBatis 관점의 데이터 객체 정책

    MyBatis는 SQL 중심적이므로, 객체는 SQL의 파라미터를 전달하거나 결과를 담는 '바구니' 역할에 집중합니다.

    - POJO/DTO 중심성: DB 테이블 구조와 거의 동일한 형태의 클래스를 주로 사용하며, 이를 보통 VO 또는 DTO라고 부르기도 합니다. (JPA만큼 엄격하게 구분하지 않는 경우가 많음)

    - 수동 매핑: SQL 쿼리 결과(resultMap)를 어떤 객체에 담을지 개발자가 직접 지정합니다.

    - 로직 분리: 객체는 데이터만 가지고 있고, 비즈니스 로직은 주로 Service 계층에서 처리하는 절차지향적 구조를 띠는 경우가 많습니다.

> MyBatis 예시 구조

```xml
<select id="findById" resultType="MemberDTO">
    SELECT id, name, point 
    FROM member 
    WHERE id = #{id}
</select>
```

```java
// MyBatis용 데이터 객체: 데이터 전달에 집중 (로직 거의 없음)
public class MemberDTO {
    private Long id;
    private String name;
    private int point;
    // Getter, Setter 위주
}
```
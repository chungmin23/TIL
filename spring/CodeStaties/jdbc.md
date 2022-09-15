## 쿼리 메서드

Spring Data JDBC에서는 ‘`find + By + SQL 쿼리문에서 WHERE 절의 컬럼명 + (WHERE 절 컬럼의 조건이 되는 데이터)` ’ 형식으로 쿼리 메서드(Query Method)를 정의하면 조건에 맞는 데이터를 테이블에서 조회합니다.

 정의한 쿼리 메서드(Query Method)는 내부적으로 아래의 SQL 쿼리문으로 변환되어 데이터베이스의 MEMBER 테이블에 질의를 보냅니다.

``SELECT "MEMBER"."NAME" AS "NAME", "MEMBER"."PHONE" AS "PHONE", "MEMBER"."EMAIL" AS "EMAIL", "MEMBER"."MEMBER_ID" AS "MEMBER_ID" FROM "MEMBER" **WHERE "MEMBER"."EMAIL" = ?**`

※ findByxxxx에서 사용하는 컬럼명은 내부적으로는 테이블의 컬럼명으로 변경되지만 Spring JDBC 입장에서는 엔티티 클래스를 바라보고 작업을 하기 때문에 반드시 **엔티티 클래스의 멤버 변수명**을 적어주어야 한다는 것입니다.

---

## @query

~~~java
public interface CoffeeRepository extends CrudRepository<Coffee, Long> {
    // (1)
    Optional<Coffee> findByCoffeeCode(String coffeeCode);

    // (2)
    @Query("SELECT * FROM COFFEE WHERE COFFEE_ID = :coffeeId")
    Optional<Coffee> findByCoffee(Long coffeeId);
}
~~~

`@Query` 애너테이션은 쿼리 메서드명을 기준으로 SQL 쿼리문을 생성하는 것이 아니라 개발자가 직접 쿼리문을 작성해서 질의를 할 수 있도록 해줍니다.

---

~~~java
     Optional.ofNullable(member.getName())
                .ifPresent(name -> findMember.setName(name));
        Optional.ofNullable(member.getPhone())
                .ifPresent(phone -> findMember.setPhone(phone));

~~~

##  **Optional.ofNullable(value)**

- null인지 아닌지 확신할 수 없는 객체를 담고 있는 Optional 객체 생성

- `Optional.ofNullable()`을 이용해서 null 값을 허용할 수 있다. 따라서 값이 null 이더라도 `NullPointerException`이 발생하지 않고, 다음 메서드인 `ifPresent()` 메서드를 호출할 수 있습니다. 수정할 값이 있다면(`name` 또는 `phone` 멤버 변수의 값이 `null`이 아니라면) `ifPresent()` 메서드내의 코드가 실행이 되고, 수정할 값이 없다면 (`name` 또는`phone` 멤버 변수의 값이 `null`이라면) 아무 동작도 하지 않습니다.

##  .ifPresent

ifPresent()는 Optional 객체가 값을 가지고 있으면 실행 값이 없으면 넘어감

---


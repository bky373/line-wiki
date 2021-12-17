# @Modifying 어노테이션

## 1. 개요
* 이번 문서에서는 스프링 데이터 JPA @Query 주석을 사용하여 업데이트 쿼리 를 생성하는 방법에 대해 배운다.
* 또한 업데이트 쿼리를 정상적으로 동작시키기 위해 @Modifying 주석을 사용하는 방법에 대해서도 배운다.  

## 2. 스프링 데이터 JPA에서 쿼리하는 방법
* 스프링 데이터 JPA가 데이터를 쿼리하기 위해 제공하는 3가지 메커니즘을 살펴보자.
    * Query 메서드
    * @Query 어노테이션
    * 커스텀 Repository 구현
* User 클래스와 스프링 데이터 JPA repository 를 만들어 위의 내용을 테스트해보자.
  ```java
    @Entity
    @Table(name = "users", schema = "users")
    public class User {
    
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private int id;
        private String name;
        private LocalDate creationDate;
        private LocalDate lastLoginDate;
        private boolean active;
        private String email;
    
    }
  ```
  ```java
    public interface UserRepository extends JpaRepository<User, Integer> {}
  ```
* 쿼리 메서드 메커니즘을 사용하면 메서드 이름에서 쿼리를 파생하여 데이터를 조작할 수 있다.
  ```java
    List<User> findAllByName(String name);
    void deleteAllByCreationDateAfter(LocalDate date);
  ```
  * 이 예에서는 이름(`name`)으로 사용자를 검색하는 쿼리 와 생성 날짜(`CreationDate`)가 특정 날짜 이후인 사용자를 제거하는 쿼리를 사용한다.
* `@Query` 어노테이션을 사용하는 경우에는, 특정 JPQL 또는 SQL 쿼리를 작성하여 사용할 수 있다.
  ```java
    @Query("select u from User u where u.email like '%@gmail.com'")
    List<User> findUsersWithGmailAddress();
  ```
  * 여기에서는 `@gmail.com` 이메일 주소를 가진 사용자를 검색하는 쿼리를 사용하였다.
* 첫 번째 메커니즘을 사용하면 원하는 데이터를 검색하거나 삭제할 수 있다. 
* 그리고 두 번째 메커니즘을 사용하면 거의 모든 쿼리를 실행할 수 있다. 그러나 **업데이트 하는 쿼리를 사용하려면 `@Modifying` 주석을 추가해야 한다.**


## 3. @Modifying 어노테이션 사용하는 방법
* [@Modifying](https://docs.spring.io/spring-data/jpa/docs/current/api/org/springframework/data/jpa/repository/Modifying.html) 어노테이션은 `@Query` 기능을 향상시킨다.
* 따라서 단순히 SELECT 쿼리 뿐 아니라 INSERT, UPDATE, DELETE 그리고 DDL 쿼리를 실행할 수 있게 해준다.
* 먼저 `@Modifying` UPDATE 쿼리 예시를 살펴보자.
  ```java
    @Modifying
    @Query("update User u set u.active = false where u.lastLoginDate < :date")
    void deactivateUsersNotLoggedInSince(@Param("date") LocalDate date);
  ```
  * 여기에서는 주어진 날짜 이후 로그인하지 않은 사용자를 비활성화한다.
* 이번에는 DELETE 쿼리 예시를 살펴보자.
  ```java
    @Modifying
    @Query("delete User u where u.active = false")
    int deleteDeactivatedUsers();
  ```
  * 여기에서는 비활성화된 사용자들을 삭제한다. 
  * 위 메서드를 조금 더 살펴보면 정수를 반환하고 있다. 이처럼 @Modifying 쿼리는 업데이트된 엔터티의 수를 제공하는 기능을 가지고 있다.
  * 주의할 점은 위와 같은 @Query DELETE 쿼리 가 스프링 데이터 JPA에서 제공하는 deleteBy~ 쿼리 메서드 와 다르게 작동한다는 점이다.
  * 후자(deleteBy~ 메서드)의 경우, 먼저 데이터베이스에서 엔티티를 가져온 다음 이를 하나씩 삭제한다. 그리고 이는 해당 엔터티 에서 수명 주기 메서드 @PreRemove 가 호출 됨을 의미한다.
  * 하지만 전자(@Query 쿼리)의 경우, 데이터베이스에 대해 단일 쿼리가 실행된다.
* 마지막으로 DDL 쿼리 를 사용 하여 삭제된 열을 USERS 테이블에 추가하자.
  ```java
    @Modifying
    @Query(value = "alter table USERS.USERS add column deleted int(1) not null default 0", nativeQuery = true)
    void addDeletedColumn();
  ```
  * 다만 이러한 방식을 사용하면 기저에 있는 영속성 컨텍스트가 구식이 되어버린다. 다음 섹션에서 이 상황을 관리하는 방법에 대해 알아보자.

### 3.1. @Modifying 을 사용하지 않을 때의 결과
* 삭제 쿼리에서 `@Modifying` 주석을 넣지 않을 때 어떤 일이 발생하는지 확인해보자.
* 먼저 `@Modifying` 을 사용하지 않는 쿼리 를 하나 만들자.
  ```java
    @Query("delete User u where u.active = false")
    int deleteDeactivatedUsersWithNoModifyingAnnotation();
  ```
* 그런 다음 위의 방법을 그대로 실행하면 `InvalidDataAccessApiUsage` 예외가 발생하는 것을 볼 수 있다.
  ```shell
    org.springframework.dao.InvalidDataAccessApiUsageException: org.hibernate.hql.internal.QueryExecutionRequestException: 
    Not supported for DML operations [delete com.baeldung.boot.domain.User u where u.active = false]
    (...)
  ```
  * 오류 메시지는 명확하게 해당 쿼리가 DML 작업에 대해 지원되지 않는다고 밝히고 있다.
  

# 참고 자료

* [Spring Data JPA @Modifying Annotation](https://www.baeldung.com/spring-data-jpa-modifying-annotation)

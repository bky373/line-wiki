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



# 참고 자료

* [Spring Data JPA @Modifying Annotation](https://www.baeldung.com/spring-data-jpa-modifying-annotation)

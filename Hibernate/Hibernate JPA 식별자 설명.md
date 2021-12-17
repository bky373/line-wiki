# Hibernate/JPA 식별자 설명

## 1. 개요
* Hibernate의 식별자(Identifiers)는 엔터티의 기본 키(primary key)를 의미한다. 
* 이는 값이 고유하여 **특정 엔터티를 식별할 수 있고, null이 아니며, 수정되지 않음** 을 의미한다.
* Hibernate는 식별자를 정의하는 몇 가지 다른 방법을 제공한다. 이번 문서에서는 라이브러리를 사용하여 엔티티 ID를 매핑하는 다양한 방법들을 검토한다.


## 2. 단순 식별자(Simple Identifiers)
* 식별자를 정의하는 가장 간단한 방법은 `@Id` 주석을 사용하는 것이다.
* 단일 속성에 `@Id` 주석을 사용할 경우 단순한 ID로 매핑 가능하다.
  * 단일 속성의 타입으로 다음과 타입을 사용할 수 있다.
    * Java Primitive Types
    * Primitive Wrapper Types 
    * String
    * Date
    * BigDecimal 및 BigInteger
* `long` 유형의 기본 키를 사용하여 엔터티를 정의하는 간단한 예를 살펴보자.
    ```java
      @Entity
      public class Student {
        
        @Id
        private long studentId;
  
        // standard constructor, getters, setters
      } 
    ``



## 3. 생성된 식별자(Generated Identifiers)
* `@GeneratedValue` 주석을 추가하여 기본 키 값을 자동으로 생성할 수 있다.
* 생성 유형은 다음 네 가지로 나뉜다. 아래 섹션에서 하나씩 살펴보기로 하자.
  * `AUTO`
  * `IDENTITY`
  * `SEQUENCE`
  * `TABLE`
* 값을 명시적으로 지정하지 않으면 생성 유형의 기본값은 `AUTO` 다.

### 3.1. AUTO 생성

* 기본(default) 생성 유형을 사용할 때, 영속성 공급자는 기본 키의 속성 타입에 따라 값을 결정한다. 이 타입은 숫자 또는 UUID일 수 있다.
* 숫자 값의 경우, 생성은 시퀀스 또는 테이블 생성기(table generator)를 기반으로 한다. UUID 값의 경우라면, UUIDGenerator를 사용한다.
* 먼저 엔터티의 기본 키를 AUTO 생성 전략을 사용하여 매핑해보자.
    ```java
      @Entity
      public class Student {
    
        @Id
        @GeneratedValue
        private long studentId;
    
        // ...
      }
    ```
  * 이 경우 기본 키는 데이터베이스 수준에서 고유한 값을 가진다.
* 이제 Hibernate 5에서 도입된 UUIDGenerator를 살펴보자.
* 이 기능을 사용하려면 `@GeneratedValue` 주석으로 UUID 유형의 ID를 선언하기만 하면 된다.
    ```java
      @Entity
      public class Course {
        
        @Id
        @GeneratedValue
        private UUID courseId;
    
        // ...
      }
    ```
  * Hibernate는 "8dd5f315-9788-4d00-87bb-10eed9eff566" 형식의 ID를 생성한다.


## 참고 자료
* [출처](https://www.baeldung.com/hibernate-identifiers)

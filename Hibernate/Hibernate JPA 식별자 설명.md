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



## 참고 자료
* [출처](https://www.baeldung.com/hibernate-identifiers)

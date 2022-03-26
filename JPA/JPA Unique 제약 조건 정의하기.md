# JPA Unique 제약 조건 정의하기

## 1. 소개
* JPA와 Hibernate를 사용하여 Unique 제약 조건을 정의하는 방법에 대해 알아본다.
* 먼저는 Unique 제약 조건과 기본 키(pk) 제약 조건의 차이를 알아본다.
* 다음으로, `@Column(unique=true)` 과 `@UniqueConstraint` 을 살펴본다. 단일(single) 컬럼과 여러(multiple) 컬럼에 대한 Unique 제약 조건을 정의하기 위해 각각의 어노테이션을 사용한다. 
* 마지막으로, 참조된 테이블 컬럼(referenced table columns)에 제약 조건을 정의하는 방법을 살펴본다. 


## 2. Unique 제약 조건
* 고유 키(Unique Key)는 데이터베이스 테이블의 레코드를 고유하게 식별하는 데 사용된다. 
* 고유 키(Unique Key)는 테이블의 단일 컬럼 또는 여러 컬럼들의 집합(Set)이다.
* **고유 키와 기본 키 제약 조건은 모두 컬럼 또는 컬럼의 집합에 대해 고유성을 보장한다는 공통점을 가지고 있다.**

### 2.1. 기본 키 제약 조건과의 차이점
* Unique 제약 조건은 컬럼 또는 컬럼 조합의 데이터가 각 행에 대해 고유하도록 한다. 예를 들어 테이블의 기본 키는 암시적 고유 제약 조건으로 작동한다. 따라서 **기본 키 제약 조건에는 이미 자동으로 고유 제약 조건이 있다고 할 수 있다.**
* 테이블 관점에서 비교하면, 기본 키 제약 조건은 테이블당 하나의 조건만 가질 수 있다. 하지만 Unique 제약 조건은 테이블당 여러 개를 가질 수 있다.
* 간단히 말해서 **기본 키 매핑에 수반되는 모든 제약 조건에 추가로 Unique 제약 조건이 적용된다.**
* 우리가 정의한 Unique 제약 조건은 **테이블 생성 중에 데이터베이스 제약 조건을 생성하는 데 사용되며, 런타임에 명령문 삽입, 업데이트 또는 삭제를 수행하는 데 사용될 수도 있다.**

### 2.2. 단일 컬럼 및 다중 컬럼 제약 조건
* Unique 제약 조건은 컬럼 제약 조건 또는 테이블 제약 조건일 수 있다. 테이블 레벨에서 여러 열에 걸쳐 Unique 제약 조건을 정의할 수 있다.
* JPA를 사용하면 `@Column(unique=true)` 및 `@UniqueConstraint` 를 사용하여 Unique 제약 조건을 정의할 수 있다. 이러한 어노테이션은 스키마 생성 프로세스에서 해석되어 자동으로 제약 조건을 생성한다.
* 무엇보다 먼저 **컬럼 레벨의 제약 조건은 단일 컬럼에 적용되고 테이블 레벨의 제약 조건은 전체 테이블에 적용된다는 점을 알아야 한다.**
* 다음 섹션의 예제를 통해 위 내용을 다시 한 번 살펴보자.


## 3. 엔티티 설정
* **JPA의 엔터티는 데이터베이스에 저장된 테이블을 나타낸다. 엔터티의 모든 인스턴스는 테이블의 행을 나타낸다.**
* 도메인 엔터티를 만들고 데이터베이스 테이블에 매핑하는 것으로 시작하자. 예제에서는 `Person` 엔터티를 사용한다.
    ```java
    @Entity
    @Table
    public class Person implements Serializable {
        @Id
        @GeneratedValue
        private Long id;  
        private String name;
        private String password;
        private String email;
        private Long personNumber;
        private Boolean isActive;
        private String securityNumber;
        private String departmentCode;
        @JoinColumn(name = "addressId", referencedColumnName = "id")
        private Address address;
       //getters and setters
     }
    ```
* 주소(address) 필드는 주소(Address) 엔터티에서 참조된 필드다.
    ```java
    @Entity
    @Table
    public class Address implements Serializable {
        @Id
        @GeneratedValue
        private Long id;
        private String streetAddress;
        //getters and setters
    }
    ```

## 4. 컬럼 제약 조건
* 모델이 준비되면 첫 번째 Unique 제약 조건을 구현할 수 있다.
* 사람의 정보를 담고 있는 Person 엔터티를 생각해보자. id 컬럼에 대한 기본 키가 있다. 또한 중복 값을 허용하지 않는 personNumber도 있다. 하지만 테이블에서 이미 기본 키를 사용 중이기 때문에 personNumber를 기본 키로 정의할 순 없다.
* 이 경우 컬럼 Unique 제약 조건을 사용하여 personNumber 필드에 중복 값을 허용하지 않도록 할 수 있다. JPA를 쓰고 있다면 `@Column` 어노테이션을 사용하자.
* 먼저는 `@Column` 어노테이션을 살펴본 다음 이를 구현하는 방법에 대해 알아보자.


### 4.1. @Column(unique=true)
* `@Column` 은 영속적인 프로퍼티 또는 필드에 대한 매핑된 열을 지정하는 데 사용된다.
* 아래 정의를 보자.
  ```java
  @Target(value={METHOD,FIELD})
  @Retention(value=RUNTIME)
  public @interface Column {
    boolean unique;
    //other elements
  }
  ```
* **`uniuqe` 속성은 컬럼이 고유 키인지 여부를 지정한다. 이는 `UniqueConstraint` 어노테이션의 shortcut이기도 하다. 고유 키 제약 조건이 단일 컬럼에만 해당할 때 유용한 속성이다.**

### 4.2. 컬럼 제약 조건 정의
* Unique 제약 조건이 단일 필드에만 해당할 때, 해당 컬럼에 `@Column(unique=true)` 을 사용할 수 있다.
* 위 예시의 personNumber 필드에 Unique 제약 조건을 정의해보자.
  ```java
    @Column(unique=true)
    private Long personNumber;
  ```
* 이렇게 하면 스키마 생성 프로세스를 실행할 때 로그에서 유효성을 검사할 수 있다.
  ```shell
    [main] DEBUG org.hibernate.SQL -
      alter table Person add constraint UK_d44q5lfa9xx370jv2k7tsgsqt unique (personNumber)
  ```
* 마찬가지로 `Person` 이 고유한 이메일을 등록하도록 제한하려는 경우 이메일 필드에 Unique 제약 조건을 추가할 수 있다.
  ```java
    @Column(unique=true)
    private String email;
  ```
* 스키마 생성 프로세스를 실행하고 제약 조건을 확인해보자.
  ```shell
    [main] DEBUG org.hibernate.SQL -
      alter table Person add constraint UK_585qcyc8qh7bg1fwgm1pj4fus unique (email)
  ```
* 때로는 컬럼의 일부 조합인 복합 키(composite key)에 Unique 제약 조건을 추가하고 싶을 수도 있다. 복합 고유 키를 정의하기 위해 테이블 제약 조건을 사용할 수 있다. 이는 다음 섹션에서 살펴보기로 하자.

## 5. 테이블 제약 조건
* [문서](https://www.baeldung.com/jpa-unique-constraints#column-constraints) 참고

## 6. 참조된 테이블 열에 대한 Unique 제약 조건
* [문서](https://www.baeldung.com/jpa-unique-constraints#column-constraints) 참고

## 7. 결론
* Unique 제약 조건은 두 레코드가 단일 컬럼 또는 컬럼 집합에서 동일한 값을 갖는 것을 방지한다.
* 특히 이번 문서에서는 JPA에서 어떻게 Unique 제약 조건을 정의하는지에 대해 알아보았다. 특히 `@Column(unique=true)` 및 `@UniqueConstraint` 어노테이션에 대해 알아보았다.

# 출처
* [Defining Unique Constraints in JPA](https://www.baeldung.com/jpa-unique-constraints)


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


### 3.2. IDENTITY 생성
* 이 유형의 생성은 IdentityGenerator에 의존한다.
* IdentityGenerator는 데이터베이스 ID 열(column)에 의해 생성되는 값을 예상하여 값을 설정한다(자동으로 증가되는 설정이다).
* 이 생성 유형을 사용하려면 전략 매개변수(strategy parameter)만 설정하면 된다.
    ```java
        @Entity
        public class Student {
        
            @Id
            @GeneratedValue (strategy = GenerationType.IDENTITY)
            private long studentId;
        
            // ...
        }
    ```
* 한 가지 주의할 점이 있다면, IDENTITY 생성은 일괄 업데이트(batch updates)를 활성화 하지 않는다(disable).


### 3.3. SEQUENCE 생성

* 시퀀스 기반의 id를 사용하기 위해 Hibernate는 SequenceStyleGenerator 클래스를 제공한다.
* 이 생성기는 데이터베이스가 시퀀스 기능 지원하는 경우에 한해 시퀀스를 사용할 수 있다. 만약 지원하지 않는 경우라면 테이블 생성(table generation) 방식을 사용한다.
* 시퀀스 이름을 커스텀하게 사용하기 위해 `@GenericGenerator` 주석과 함께 SequenceStyleGenerator 전략을 사용할 수 있다.
    ```java
        @Entity
        public class User {
            @Id
            @GeneratedValue(generator = "sequence-generator")
            @GenericGenerator(
                name = "sequence-generator",
                strategy = "org.hibernate.id.enhanced.SequenceStyleGenerator",
                parameters = {
                  @Parameter(name = "sequence_name", value = "user_sequence"),
                  @Parameter(name = "initial_value", value = "4"),
                  @Parameter(name = "increment_size", value = "1")
                }
            )
            private long userId;
            
            // ...
        }
    ```
  * 이 예에서는 시퀀스의 초기 값도 설정했는데, 이는 기본 키 생성이 4에서 시작됨을 의미한다.
  * SEQUENCE는 Hibernate 문서에서 권장하는 생성 유형이다.
  * 생성된 값은 시퀀스별로 고유하다. 시퀀스 이름을 지정하지 않으면 Hibernate는 다른 유형에 동일한 hibernate_sequence를 재사용한다.



### 3.4. TABLE 생성
* TableGenerator는 식별자 생성 값의 세그먼트를 유지하는 기본 데이터베이스 테이블을 사용한다.
* `@TableGenerator` 주석을 사용하여 테이블 이름을 커스텀하게 지정할 수 있다.
  ```java
    @Entity
    public class Department {
      @Id
      @GeneratedValue(
        strategy = GenerationType.TABLE,
        generator = "table-generator"
      )
      @TableGenerator(
        name = "table-generator",
        table = "dep_ids",
        pkColumnName = "seq_id",
        valueColumnName = "seq_value"
      )
      private long depId;
      
      // ...
    }
  ```
  * pkColumnName 및 valueColumnName과 같은 다른 속성도 커스텀하게 지정할 수 있다.
  * 그러나 이 방법은 확장에 유리하지 않고 성능에 부정적인 영향을 줄 수 있다는 단점이 있다.

* 요약하여 말하자면 위의 4가지 생성 유형 방식은 서로 유사한 값이 생성되지만 다른 데이터베이스 메커니즘을 사용한다고 할 수 있다.

### 3.5. Custom 생성
* 기본 생성 전략을 사용하고 싶지 않다면, IdentifierGenerator 인터페이스를 구현하여 커스텀한 생성기를 사용할 수 있다.
* String 접두사와 숫자를 포함한 식별자를 생성하는 생성기를 만들어보자.
  ```java
    public class MyGenerator implements IdentifierGenerator, Configurable {

      private String prefix;

      @Override
      public Serializable generate(
        SharedSessionContractImplementor session, Object obj) 
        throws HibernateException {
  
          String query = String.format("select %s from %s", 
              session.getEntityPersister(obj.getClass().getName(), obj)
                .getIdentifierPropertyName(),
              obj.getClass().getSimpleName());
  
          Stream ids = session.createQuery(query).stream();
  
          Long max = ids.map(o -> o.replace(prefix + "-", ""))
            .mapToLong(Long::parseLong)
            .max()
            .orElse(0L);
  
          return prefix + "-" + (max + 1);
      }
  
      @Override
      public void configure(Type type, Properties properties, 
          ServiceRegistry serviceRegistry) throws MappingException {
              prefix = properties.getProperty("prefix");
      }
    }
  ```
  * 이 예에서는 IdentifierGenerator 인터페이스의 `generate()` 메서드를 재정의한다.
  * 먼저, `{prefix}-XX` 형식의 기존 기본 키에서 가장 높은 수를 찾는다. 그런 다음 가장 높은 수에 1을 추가하고 접두사 속성을 추가하여 새로운 id 값을 얻는다.
  * 추가적으로 Configurable 인터페이스를 구현하였기 때문에 `configure()` 메서드에서 접두사 속성 값을 설정할 수 있다.
  * 다음으로 이 커스텀 생성기를 엔터티에 추가해 보도록 하자.
  * 이를 위해 `@GenericGenerator` 주석과 함께 생성기 클래스의 전체 클래스 이름을 포함하는 전략 매개변수를 작성한다.
    ```java
      @Entity
      public class Product {
      
          @Id
          @GeneratedValue(generator = "prod-generator")
          @GenericGenerator(name = "prod-generator", 
            parameters = @Parameter(name = "prefix", value = "prod"), 
            strategy = "com.baeldung.hibernate.pojo.generator.MyGenerator"
          )
          private String prodId;
      
          // ...
      }
    ```
    * 여기에서는 접두사 매개변수를 "prod"로 설정하였다.
    * 생성된 id 값을 더 명확하게 이해하기 위해 간단히 JUnit 테스트를 수행해본다.
      ```java
        @Test
        public void whenSaveCustomGeneratedId_thenOk() {
            Product product = new Product();
            session.save(product);
            Product product2 = new Product();
            session.save(product2);
        
            assertThat(product2.getProdId()).isEqualTo("prod-2");
        }
      ```
      * 여기에서 "prod" 접두사를 사용하여 생성된 첫 번째 값은 "prod-1"이 되고 그 뒤에 생성된 값은 "prod-2"가 된다.


## 참고 자료
* [출처](https://www.baeldung.com/hibernate-identifiers)
* [[JPA] 식별자 할당 SEQUENCE(시퀀스) 사용 전략](https://dololak.tistory.com/479)

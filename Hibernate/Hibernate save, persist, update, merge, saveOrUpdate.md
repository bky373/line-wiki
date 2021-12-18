# Hibernate: save, persist, update, merge, saveOrUpdate

## 1. 개요
* Session 인터페이스에서 지원하는 다음 메소드들 간 차이에 대해 알아본다.
    * save
    * persist
    * update
    * merge
    * saveOrUpdate


## 2. 영속성 컨텍스트의 구현; 세션(Session as a Persistence Context Implementation)
* [Session](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Session.html) 인터페이스는 데이터베이스에 데이터를 저장하는 메서드를 여러 개 지원한다.
    > save, persist, update, merge, saveOrUpdate
* 이 메서드들 간의 차이를 이해하기 위해서 먼저 (영속성 컨텍스트로서 존재하는) 세션의 목적에 대해 이해해야 한다.
* 그리고 세션과 관련 있는 엔티티 인스턴스들의 상태 차이에 대해서도 이해해야 한다.

### 2.1. 엔티티 인스턴스 관리하기
* Hibernate는 객체-관계 매핑을 지원하기도 하지만, **런타임 동안 엔티티를 관리하는 문제** 에도 관심이 있다. 
* 그리고 이러한 문제를 해결하기 위해 **영속성 컨텍스트(Persistence Context)** 라는 개념을 도입하였다.
* 영속성 컨텍스트는 **세션 중에 데이터베이스에 로드하거나 저장한 모든 개체에 대한 컨테이너 또는 first-level 캐시**로 이해할 수 있다.
* 참고로, **세션은 비즈니스 로직에 의해 경계가 정의되는 논리적 트랜잭션** 이다. 
* 영속성 컨텍스트를 통해 데이터베이스 작업을 하고, 모든 엔티티 인스턴스가 이 컨텍스트에 연결되어 있으면, 세션 중 사용하는 데이터베이스 레코드 각각에 대응되는 단일 엔티티 인스턴스가 항상 있어야 한다. 
* Hibernate 에서 영속성 컨텍스트는 `org.hibernate.Session` 인스턴스로 표현된다. JPA의 경우에는, `javax.persistence.EntityManager` 이다.
* Hibernate를 JPA 공급자로 사용하고 EntityManager 인터페이스를 통해 작업할 때 이 인터페이스의 구현은 기본적으로 기저의 Session 객체를 래핑 한다.
* Hibernate Session 의 경우 더 풍부한 인터페이스를 제공하기 때문에 Session 을 직접 사용하는 것이 더 유용할 때도 있다.

### 2.2. 엔티티 인스턴스의 상태
* 애플리케이션의 모든 엔티티 인스턴스는 세션 영속성 컨텍스트 와 관련하여 아래 세 가지 주요 상태 중 하나를 갖는다.
  * **일시적(`transient`)**: 이 인스턴스는 이전에 세션에 연결된 적이 없고, 현재도 연결되어 있지 않다. 또한 데이터베이스에 해당 인스턴스와 대응되는 행이 존재하지 않는다. 일반적으로 데이터베이스에 저장하기 위해 새로 만들어진 개체가 여기에 해당한다.
  * **영속적(`persistent`)**: 이 인스턴스는 고유한 세션 객체와 연결된다. 세션을 데이터베이스로 플러시(flush)할 때 이 엔티티는 데이터베이스에 대응되는 일관된 레코드가 있음이 보장된다.
  * **분리된(`detached`)**: 이 인스턴스는 한 번 (영속적인 상태에서) 세션에 연결된 적이 있지만 지급은 그렇지 않은 경우에 해당한다. 컨텍스트에서 인스턴스를 퇴출하거나(evict), 세션을 지우거나 닫는 경우, 직렬화/역직렬화 프로세스를 통해 인스턴스를 넣는 경우, 인스턴스는 이 상태를 갖는다. 
* 다음의 상태 다이어그램(state diagram)은 세 가지 인스턴스 상태와 이들 상태 간 전환을 일으키는 세션 메서드들을 적절한 위치에 두어 표현하고 있다. 
  <img width="1034" alt="image" src="https://user-images.githubusercontent.com/49539592/146630105-9922ed84-4627-4084-9de3-f12300013ce7.png">
* 엔티티 인스턴스가 영속적(persistent) 상태에 있을 때, 이 인스턴스가 가진 매핑 필드의 모든 변경사항은 세션 플러시 시 해당 데이터베이스 레코드와 필드에 반영된다.
* 영속적(persistent) 인스턴스는 "온라인" 상태로 간주되는 한편, 분리된(detached) 인스턴스는 "오프라인"으로 간주되어 변화를 모니터링하지 않게 된다.
* 이는 영속적(persistent) 객체의 필드 변경 후 이를 데이터베이스에 적용하기 위해 save 나 update 와 같은 메서드를 호출할 필요가 없다는 것을 의미한다.
* 대신 변경 작업이 완료되었을 때 트랜잭션을 커밋하거나 세션을 플러시하거나 닫아주면 된다.

### 2.3. JPA 사양 준수하기
* Hibernate 는 가장 성공적인 Java ORM 구현이기 때문에 JPA(Java Persistence API) 사양이 Hibernate API의 영향을 많이 받는 것은 당연한 일이다.
* 아쉬운 점은 둘이 완전히 동일하지 않다는 사실이고, 때때로 많은 차이점을 가지고 있다는 점이다.
* 특히 Hibernate API 는 JPA 표준 구현으로 동작하기 위해 손을 조금 봐야 했다. EntityManager 인터페이스와 매칭되게끔 Session 인터페이스에 몇몇 메서드들이 추가되었다.
* 이러한 메서드들은 본래 메서드의 목적을 동일하게 수행하지만, 주어진 사양을 준수해야 했기 때문에 몇 가지 차이점을 만들었다. 이에 대해선 다음 섹션에서 살펴보자.


## 3. 작업 간 차이점
* 가장 먼저, 모든 메서드(save, persist, update, merge, saveOrUpdate)가 SQL UPDATE 또는 INSERT 문을 즉시 생성하지 않는다는 것을 이해해야 한다.
* **데이터베이스에 실제 데이터가 저장되는 때는 트랜잭션을 커밋하거나, 세션을 플러시 할 때다.**
* 그렇다면 메서드의 역할은 무엇일까?
* 위에서 언급한 **메서드들은 수명주기를 따라 상태 전환을 시키며 엔티티 인스턴스의 상태를 관리하는 역할을 한다.**
* 다음은 단순히 @Entity 어노테이션을 붙여놓은 Person 엔티티 예제다.
  ```java
    @Entity
    public class Person {
    
        @Id
        @GeneratedValue
        private Long id;
    
        private String name;
    
        // ... getters and setters
    
    }
  ```

### 3.1. Persist
* **`persist` 메서드는 영속성 컨텍스트에 새로운 엔티티 인스턴스를 추가하는 데 사용된다.**
* 즉, 인스턴스를 **일시적(`transient`) 상태에서 영속적(`persistent`) 상태로 전환** 한다.
* `persist` 메서드는 일반적으로 데이터베이스에 새로운 레코드를 추가하고 싶을 때 사용한다.
  ```java
    Person person = new Person();
    person.setName("John");
    session.persist(person);
  ```
* 위에서 `persist` 메서드가 호출된 후에는 어떤 일이 일어날까? 앞서 말했듯이 `person` 개체의 상태가 일시적(`transient`)에서 영속적(`persistent`)로 전환된다.
* 개체는 현재 영속성 컨텍스트 안에 있지만, 아직 데이터베이스에 저장되지 않은 상태다. 트랜잭션을 커밋하거나 세션을 플러시하거나 닫을 때만 INSERT 문이 생성되기 떄문이다.
* 참고로 `persist` 메서드는 `void` 반환 타입을 가지고, 여기에서 `person` 변수는 실제 영속된 객체를 참조한다.
* 이 메서드는 나중에 Session 인터페이스에 추가된다. 이 메서드의 차별화된 특징은 JSR-220 사양(EJB persistence)을 준수한다는 것이다. 이 메서드의 시맨틱(semantices)은 해당 사양에 엄격하게 정의되어 있고, 그중 몇 가지를 살펴보면 아래와 같다.
  * 일시적인(`transient`) 인스턴스는 영속적이게(`persistent`) 된다(그리고 해당 작업은 cascade=PERSIST 또는 cascade=ALL의 관계에서 단계적으로 진행된다).
  * 인스턴스가 이미 영속적인(`persistent`) 경우, 이 호출은 특정 인스턴스에 대해 영향을 주지 않는다(그러나 여전히 cascade=PERSIST 또는 cascade=ALL의 관계에서는 작업이 단계적으로 진행된다).
  * 인스턴스가 분리된(`detached`) 상태일 경우, 이 메서드를 호출하거나, 세션을 커밋하거나 플러시할 때 예외를 던질 수 있다.
* 여기에는 인스턴스의 식별자와 관련된 내용이 없다. 즉, 스펙에는 ID 생성 전략에 관계없이 ID가 즉시 생성된다는 내용이 포함되어 있지 않다.
* 결국 `persist` 메서드의 스펙을 놓고 보면, 구현에서 커밋 또는 플러시 시 id를 생성하는 문을 실행할 수 있다. 그리고 이 메서드를 호출한 후에는 id가 null이 아니라는 점이 보장되지 않는다.
* 또한 스펙에서는 분리된(`detached`) 상태의 인스턴스에 `perisist` 메서드를 호출할 경우 에외를 던질 수 있다고 했다.
* 다음 예시는 이에 대한 예시로, 하나의 엔티티를 `persist` 하고, 이를 컨텍스트에서 `evict` 한 후, 다시 이를 `perist`하는 과정을 포함한다. 결론은 알다시피 두 번째 `session.persist()` 호출에서 예외가 발생할 것이다.
  ```java
    Person person = new Person();
    person.setName("John");
    session.persist(person);
    
    session.evict(person); // detached 상태가 되었다
    
    session.persist(person); // PersistenceException!
  ```

### 3.2. Save
* **`save` 메소드는 JPA 사양을 따르지 않는 "원래" Hibernate 메소드** 다.
* 그 목적은 기본적으로 `persist` 메서드와 동일하지만 세부적인 구현 사항이 다르다. 
* 메서드 설명 문서에는, "먼저 생성된 식별자를 할당"하여 인스턴스를 영속(persist)한다고 엄격하게 명시되어 있다. 
* 이 메서드는 식별자의 Serializable 값이 반환되는 것이 보장된다.
  ```java
    Person person = new Person();
    person.setName("John");
    Long id = (Long) session.save(person);
  ```
* 이미 영속된 인스턴스를 저장하는 효과는 `persist` 와 동일하다. 차이점은 분리된(`detached`) 인스턴스를 저장하려고 할 때 발생한다.
  ```java
    Person person = new Person();
    person.setName("John");
    Long id1 = (Long) session.save(person);
    
    session.evict(person); // detached 상태가 되었다
  
    Long id2 = (Long) session.save(person);
  ```
* id2 변수는 id1과 다르다. **분리된(`detached`) 인스턴스에 대한 `save` 호출은 새로운 영속 인스턴스를 생성하고 새로운 식별자를 할당한다.** 그 결과 커밋 또는 플러시 시 **데이터베이스에 중복 레코드가 생성된다.**


## 참고 자료
* [Hibernate: save, persist, update, merge, saveOrUpdate](https://www.baeldung.com/hibernate-save-persist-update-merge-saveorupdate)


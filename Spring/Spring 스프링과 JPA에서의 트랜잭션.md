# 스프링과 JPA에서의 트랜잭션

## 1. 개요
* **스프링 트랜잭션을 구성하는 올바른 방법** 을 배웁니다.
* `@Transactional` 어노테이션을 사용하는 방법과 마주하기 쉬운 함정들(pitfall)에 대해 살펴봅니다.
* 핵심 영속성 구성(core persistence configuration)에 대해 깊이 공부하고 싶다면 [Spring with JPA](https://www.baeldung.com/the-persistence-layer-with-spring-and-jpa) 튜토리얼을 참고 바랍니다.
* 기본적으로 트랜잭션, 어노테이션 및 AOP를 구성하는 방법은 두 가지가 있으며 각각의 방법은 고유한 장점을 가지고 있습니다. 여기에서는 보다 일반적으로 사용되는 어노테이션 구성에 대해 살펴봅니다.


## 2. 트랜잭션 구성(Configuration)
* 스프링 3.1은 트랜잭션 지원을 활성화(enable)하기 위해 `@Configuration` 클래스에서 사용하는 `@EnableTransactionManagement` 어노테이션을 도입하였습니다.
    ```java
      @Configuration
      @EnableTransactionManagement
      public class PersistenceJPAConfig{
    
        @Bean
        public LocalContainerEntityManagerFactoryBean entityManagerFactoryBean() {
          //...
        }
        
        @Bean
        public PlatformTransactionManager transactionManager() {
            JpaTransactionManager transactionManager = new JpaTransactionManager();
            transactionManager.setEntityManagerFactory(entityManagerFactoryBean().getObject());
            return transactionManager;
            }
      }
    ```
* 만약 **스프링 부트 프로젝트이고, 클래스 경로(classpath)에 `spring-data-*` 또는 `spring-tx` 종속성을 가지고 있다면, 트랜잭션 관리(transaction management)가 기본적으로 활성화됩니다.**


## 3. XML로 트랜잭션 구성하기
* 3.1 이전 버전의 경우 또는 자바 기반 구성이 아닌 경우 `annotation-driven` 과 네임스페이스 지원(namespace support)을 사용하여 XML 구성을 할 수 있습니다.
  ```xml
    <bean id="txManager" class="org.springframework.orm.jpa.JpaTransactionManager">
       <property name="entityManagerFactory" ref="myEmf" />
    </bean>
    <tx:annotation-driven transaction-manager="txManager" />
  ```


## 4. 4. @Transactional 어노테이션
* **트랜잭션이 구성되면**, 이제 클래스 또는 메서드 수준에서 빈에 `@Transactional` 어노테이션을 달 수 있습니다.
  ```java
    @Service
    @Transactional
    public class FooService {
      //...
    }
  ```
* 어노테이션은 추가적인 구성 또한 지원합니다.
  * 트랜잭션의 전파 유형 (the Propagation Type of the transaction)
  * 트랜잭션의 격리 수준 (the Isolation Level of the transaction)
  * 트랜잭션에 의해 래핑된(wrapped) 작업 Timeout
  * readOnly 플래그 – 영속성 공급자에게 읽기 전용 트랜잭션임을 알려줍니다.
  * 트랜잭션에 대한 롤백 규칙 (the Rollback rules)
* 기본적으로 롤백은 런타임에 발생하며 unchecked 예외만 발생합니다. 
* **checked 예외는 트랜잭션의 롤백을 유발하지 않습니다.** 
* 물론 `rollbackFor` 와 `noRollbackFor` 어노테이션 매개변수를 사용하여 이 동작을 구성할 수 있습니다.


## 5. 잠재된 함정(Potential Pitfalls)

### 5.1. 트랜잭션과 프록시
* 상위 레벨에서 스프링은, **클래스 또는 메소드 중 하나에서 `@Transactional` 어노테이션이 달린 모든 클래스에 대한 프록시를 생성** 합니다. 
* 프록시를 통해 프레임워크는 실행 중인 메서드 전후에 트랜잭션 로직을 주입하는데, 주로 트랜잭션을 시작하고 커밋하기 위함입니다.
* 중요한 점은 트랜잭션 빈이 인터페이스를 구현하는 경우 프록시는, 기본적으로 자바 동적 프록시(Java Dynamic Proxy)가 된다는 점입니다. 
* 이는 프록시를 통해 들어오는 바깥의 메서드 호출만 가로채서 사용한다는 것을 의미합니다. 
* 반면, 메서드에 `@Transactional` 어노테이션이 달려 있어도 자체 호출은 트랜잭션을 시작하지 않는다는 것을 의미합니다.
* 프록시를 사용할 때 또 다른 주의 사항은 **public 메서드에만 `@Transactional` 어노테이션을 달아야 한다** 는 것입니다. 
* **다른 가시성의 메소드는 프록시되지 않기 때문에 어노테이션을 무시합니다.**

### 5.2. 격리 수준(Isolation Level) 변경
* 아래 방법으로 `@Transactional` 어노테이션에서 트랜잭션 격리 수준을 변경할 수 있습니다.
  ```java
    @Transactional(isolation = Isolation.SERIALIZABLE)
  ```
* 단, 해당 기능은 스프링 4.1에 도입되었습니다. 그 이전 버전에서 기능을 사용하면 다음과 같은 결과가 나타납니다.
  ```shell
    org.springframework.transaction.InvalidIsolationLevelException: Standard JPA does not support custom isolation levels 
    – use a special JpaDialect for your JPA implementation
  ```

### 5.3. 읽기 전용(Read-Only) 트랜잭션
* `readOnly` 플래그는 JPA와 함께 작업할 때 특히 혼란을 야기하는데, Javadoc에서는 다음과 같이 말합니다.
  ```
    This just serves as a hint for the actual transaction subsystem; it 
    will not necessarily cause failure of write access attempts. 
    A transaction manager which cannot interpret the read-only hint 
    will not throw an exception when asked for a read-only transaction.
    
    이것은 실제 트랜잭션 하위 시스템에 대한 힌트 역할을 합니다. 
    반드시 쓰기 액세스 시도가 실패하는 것은 아닙니다. 
    읽기 전용 힌트를 해석할 수 없는 트랜잭션 관리자는 읽기 전용 트랜잭션을 요청할 때 예외를 throw하지 않습니다.
  ```
* 중요한 점은 **`readOnly` 플래그가 설정될 때 삽입 또는 업데이트가 발생하지 않을 것이라고 확신할 수 없다** 는 점입니다. 
* 이 동작이 공급업체에 따라 다르긴 하지만 JPA는 공급업체를 알지 못하기 때문에 여전히 주의해야 합니다.
* `readOnly` 플래그가 **트랜잭션 내에서만 관련이 있다는 사실** 도 중요합니다. 
* 트랜잭션 컨텍스트 외부에서 작업이 발생하면 플래그는 단순히 무시됩니다. 
* 트랜잭션이 아닌(non-transactional) 컨텍스트에서는 트랜잭션이 생성되지 않고 `readOnly` 플래그가 무시됩니다.


### 5.4. 트랜잭션 로깅
* 트랜잭션 관련 문제를 이해하는 데 유용한 방법은 트랜잭션 패키지에서 로깅을 조정(fine-tuning)하는 것입니다. 
* 스프링 내 관련 패키지는 `org.springframework.transaction` 이며, 로깅 수준은 `TRACE` 로 설정해야 합니다.


### 5.5. 트랜잭션 롤백
* `@Transactional` 어노테이션은 하나의 메타데이터로, 메소드에 트랜잭션 시맨틱(semantics)을 지정할 수 있습니다. 
* 트랜잭션 롤백 과 관련해서는 두 가지 방법이 있습니다.
  * **선언적(declarative)**
  * **프로그래밍(programmatic)**
  
#### 5.5.1. 선언적 접근 방식
* **선언적 접근 방식에서는, 롤백을 위해 메서드에 `@Transactional` 어노테이션을 달아줍니다.** 
* `@Transactional` 어노테이션은 `rollbackFor` 또는 `rollbackForClassName` 속성을 사용하여 트랜잭션을 롤백할 수 있습니다.
* 반면 `noRollbackFor` 또는 `noRollbackForClassName` 속성을 사용하여 예외 (목록)에 대해 롤백을 방지할 수 있습니다.
* 선언적 접근 방식에서 디폴트 롤백 동작은 런타임 예외에서 롤백하는 것입니다.
* 다음은 선언적 접근 방식을 사용한 간단한 예입니다.
  ```java
    @Transactional
    public void createCourseDeclarativeWithRuntimeException(Course course) {
      courseDao.create(course);
      throw new DataIntegrityViolationException("선언적 방식의 롤백에서 예외를 던집니다.");
    }
  ```
* 이번에는 checked 예외 목록에 대해 롤백하도록 설정합니다. 
* 이번 예에서는 SQLException을 사용합니다.
  ```java
    @Transactional(rollbackFor = { SQLException.class })
    public void createCourseDeclarativeWithCheckedException(Course course) throws SQLException {
      courseDao.create(course);
      throw new SQLException("선언적 방식의 롤백에서 예외를 던집니다.");
    }
  ```
* 다음으로 `noRollbackFor` 속성을 사용한 예시를 살펴보겠습니다.
* 해당 속성 값에 SQLException을 지정하여 트랜잭션 롤백을 방지한 예시입니다.
  ```java
    @Transactional(noRollbackFor = { SQLException.class })
    public void createCourseDeclarativeWithNoRollBack(Course course) throws SQLException {
      courseDao.create(course);
      throw new SQLException("선언적 방식의 롤백에서 예외를 던집니다.");
    }
  ```
#### 5.5.2. 프로그래밍 방식
* **프로그래밍 방식에서는 `TransactionAspectSupport`를 사용하여 트랜잭션을 롤백합니다.**
  ```java
    public void createCourseDefaultRatingProgramatic(Course course) {
      try {
        courseDao.create(course);
      } catch (Exception e) {
        TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
      }
    }
  ```
* **프로그래밍 방식보다는 선언적 방식으로 롤백할 것을 권장합니다.**


## 참고 자료
* [Transactions with Spring and JPA](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring)

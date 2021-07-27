# Spring

- [Spring](https://github.com/bky373/line-wiki/blob/main/Spring.md#Spring)

- [JPA](https://github.com/bky373/line-wiki/blob/main/Spring.md#JPA)

- [Annotations](https://github.com/bky373/line-wiki/blob/main/Spring.md#Annotations)

- [Spring Cloud](https://github.com/bky373/line-wiki/blob/main/Spring.md#Spring-Cloud)

  <br>

- **스프링 (프레임워크)** &nbsp;( `Spring (Framework)` ) 

  - 스프링이란 IoC와 AOP를 지원하는 경량의 컨테이너 프레임워크이다. ( [출처](https://devlog-wjdrbs96.tistory.com/165?category=882236) )
  - 스프링 프레임워크 ( `Spring Framework` ) : 최신 **Java 기반 엔터프라이즈 애플리케이션**을 위한 **포괄적인 프로그래밍과 구성 모델**을 제공한다. 모든 종류의 배포 플랫폼에서 이용 가능하다.  ( [출처](https://spring.io/projects/spring-framework) )

  > The Spring Framework provides a comprehensive programming and configuration model for modern Java-based enterprise applications - on any kind of deployment platform.

- **스프링 부트** ( `Spring Boot` ) : 스탠드 얼론한 스프링 기반의 애플리케이션을 프로덕션 수준까지 쉽게 만들고 실행할 수 있게 해준다. 대부분의 스프링 부트 애플리케이션은 최소한의 스프링 구성을 필요로 한다.  ( [출처](https://spring.io/projects/spring-boot) )

  > Spring Boot makes it easy to create stand-alone, **production-grade Spring based Applications** that you can "just run".

- **스프링 부트의 특징** ( `Features` )   ( [출처](https://spring.io/projects/spring-boot) )

  - **자립형 ( 독립형 )  Spring 애플리케이션**을 생성함

  - **`Tomcat`,  `Jetty` 또는 `Undertow `** 를 직접 포함함  ( `WAR` 파일을 배포할 필요 없음 )

  - 빌드 구성을 단순화하기 위해 **독자적인** **'스타터' 종속성**을 제공함

  - 가능할 때마다 **`Spring` 및 타사 라이브러리**를 자동으로 구성함

  - **메트릭, 헬스 체크 ( 상태 확인 ) 및 외부 구성** 과 같은 프로덕션 준비 기능을 제공함

  - **코드 생성 및 XML 구성 요구 사항이 없음**

    > - Create stand-alone Spring applications
    > - Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
    > - Provide opinionated 'starter' dependencies to simplify your build configuration
    > - Automatically configure Spring and 3rd party libraries whenever possible
    > - Provide production-ready features such as metrics, health checks, and externalized configuration
    > - Absolutely no code generation and no requirement for XML configuration

- **빈** ( `Bean` ) : 스프링 IoC 컨테이너가 관리하는 객체 ( [출처](https://devlog-wjdrbs96.tistory.com/165) )

  - 빈으로 등록됐을 때의 장점
    - 스프링 IoC 컨테이너에 등록된 Bean 들은 **의존성 관리**가 수월해진다.
    - 스프링 IoC 컨테이너에 등록된 Bean 들은 **싱글톤** 의 형태이다.
    - 스프링 부트는 어노테이션을 통해 Bean 을 설정하고 주입받는 것을 표준으로 삼는다.

- **DI**( `Dependency Injection` ) : DI에서 의존성은 의존 관계를 의미한다. **의존 관계**란 둘 이상의 객체가 **서로 협력**하는 방법에 대한 것이다.

  > ( 의존 관계의 예 ) <br>
  > A는 **B에 의존** = A는 **B를 사용** = **B의 변화가 A에 영향을 미침** <br>
  > 즉, B가 계속해서 변한다면 **관리가 반드시 필요**하다! <br> <br>
  > ( 구체적인 사례 ) <br>
  > `Controller`는 **`Repository`에 의존**한다.  <br> 즉, `Repository` 객체를 생성하는 책임을 `Controller`가 가지는데, 이때 둘 간의 의존 관계가 생긴다. 그리고 의존 관계가 생기면 **변경할 때마다 관리**가 필요하다. 관리할 게 많아지다 보면 **에러 발생 위험**도 덩달아 많아진다. <br><br> 그런데 만약, `Controller`에 `Repository`를 연결하는 과정을 **다른 곳**에서 할 수 있다면 어떨까? 우선 개발자가 관리하는 일이 줄어들 것이다. 코드 변경에 따르는 에러 발생도 자연히 줄어들 것이다. 뿐만 아니라 개발자는 에러에 구애받지 않고 **객체를 다양하게 변경**할 수 있게 된다. 스프링에서는 이러한 유익을 위해  **Spring IoC Container**을 제공하고 있다. <br><br>
  >
  > 결국 DI는 객체 간의 **의존 관계**를 객체가 아닌 **스프링에서 직접 연결**해주는 것을 의미한다. 그리고 이를 위해 **`@Component`** 와   **`@Autowired`**  어노테이션을 지원한다.

- **DAO** ( `Data Access Object` )   ( [출처](https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html) )

  - 실제로 DB에 접근하는 객체으로, Persistence Layer (**DB에 data를 CRUD하는 계층**) 이다. 

  - `Service`와 `DB`를 연결하는 고리의 역할을 한다.

  - 개발자가 직접 코딩한 SQL을 사용하여 DB에 접근한 후 적절한 CRUD API를 제공한다.

    - JPA에서 대부분 기본적인 CRUD 메소드를 제공하고 있다. ( `extends JpaRepository<User, Long>` )

  - 예시( `JPA` 사용 시 )

    ```java
    public interface SampleRepository extends CrudRepository<Sample, Long> {
    
    }
    ```

- **데이터 전송 객체** ( `Data Transfer Object`, &nbsp; **DTO** ) :  **계층( `Layer` ) 간 데이터 교환을 위해 사용하는 객체** 이다. 로직을 가지고 있지 않은 **순수한 객체** 이며, **`getter/setter` 메소드만** 을 갖는다. 쉽게, 데이터를 전송할 때 사용하는 바구니라고 할 수 있다. ( [출처](https://www.youtube.com/watch?v=J_Dr6R0Ov8E) )

  - `Entity`를 `DTO` 대신 사용할 수 있지 않을까?

    - 사용할 수는 있지만.. `View`에서 표현하는 속성 값들이 요청에 따라 계속 달라질 수 있는데, 그 때마다 `Entity`의 속성값을 변경하면 영속성 모델을 표현한 `Entity`의 순수성이 모호해지기 때문에 `Controller` 에서 쓸 `DTO`와 `Entity`  클래스는 분리하는 게 좋다.

    > **Entity** : **실제 DB의 테이블과 매핑되는 클래스**로,  **`id`로 구분** 되며, 로직을 포함할 수 있다.

  - client( browser ) <- **dto** -> controller( web ) - service - repository ( dao ) <- **domain(entity)** -> DB

- **MockMvc** : 웹 애플리케이션을 애플리케이션 서버에 배포하지 않고도 스프링 MVC의 동작을 재현할 수 있는 클래스이다. ( [출처](https://itmore.tistory.com/entry/MockMvc-%EC%83%81%EC%84%B8%EC%84%A4%EB%AA%85) )

- **Dispatcher Servlet** : 모든 **요청을 한곳에서 받아서 필요한 처리**들을 한 뒤, **요청에 맞는 handler로 요청을 Dispatch**하고, 해당 **Handler의 실행 결과를 Http Response 형태로 만드는 역할**을 한다. ( [출처](https://galid1.tistory.com/525) )

- **MyBatis** : **XML로 쿼리를 작성** 하도록 지원하는 라이브러리다. 참고로 `mybatis-spring` 은 스프링과 `mybatis` 를 연동해주는 라이브러리이다.

## JPA

- **ORM** (`Object Relational Mapping`) : 객체 관계 매핑으로, 객체와 관계형 데이터베이스의 데이터를 매핑(연결)해주는 것을 말한다. 객체를 ORM 프레임워크에 저장하면, ORM 프레임워크가 SQL을 생성해서 DB에서 객체를 관리할 수 있게 해준다.

- **JPA** (`Java Persistence API`) : 자바 진영의 ORM 기술 표준이다.

- **JPA `persist`, `flush`, `clear` 정의 및 이해**  &nbsp;( [출처](https://apalsl.github.io/2019/11/14/SpringJpaFlushClear/) )

  ```java
   public Long save(Member member) {
       // 영속성 컨텍스트에 저장
       em.persist(member);
       
       // 영속성 컨텍스트에 있는 데이터를 DB로 쿼리 전송 
       // DB에 실제 쿼리를 날리지만 DB에 반영되지는 않는다. 
       // DB에 저장하기 위해선 commit을 해야 한다.
       em.flush(); 
       
       // 영속성 컨텍스트에 있는 데이터를 제거
       em.clear(); 
       
       return member.getId();
   }
  ```

## Annotations

- **@RequestParam** : 1개의 **HTTP 요청 파라미터** ( `Path Parameter` 또는 `Query Parameter` )를 받기 위해 사용한다. 사용시 필수 여부 ( `required` ) 의 기본값이 `true` 이기 때문에, 사용하려면 반드시 해당 파라미터가 전송되어야 한다. 해당 파라미터가 전송되지 않으면 `400 Error` 가 발생한다. 만약 반드시 필요한 변수가 아니라면 `required`의 값을 `false` 로 설정한다. 해당 변수가 없는 요청을 보낼 경우에 기본값을 설정하고 싶다면 `defaultValue` 값을 설정한다.  ( [출처](https://mangkyu.tistory.com/72) ) 

- **@RequestBody** : 클라이언트가 전송하는 **JSON** ( `application/json` ) 형태의 **HTTP Body 내용**을 **Java Object** 로 변환해주는 역할을 한다. 만약 Body가 존재하지 않는 `GET` 메소드에 `@RequestBody` 를 활용하려고 하면 에러가 발생한다. `@RequestBody` 로 받는 데이터는 스프링에서 관리하는 `MessageConverter` 중 하나인 `MappingJackson2HttpMessageConverter` 를 통해 **Java Obejct** 로 변환된다.  ( [출처](https://mangkyu.tistory.com/72) )

- **@ModelAttribute** : 클라이언트가 전송하는 `multipart/form-data` 형태의 **HTTP Body 내용과 HTTP 파라미터들을 Setter 를 통해 1대1로 객체에 바인딩** 하기 위해 사용한다. `@ModelAttribute` 에는 매핑시키는 파라미터의 타입이 객체의 타입과 일치하는지를 포함한 다양한 검증 ( `Validation` ) 작업이 추가적으로 진행된다. 한 가지 예로, 게시물의 번호를 저장하는 `int` 형 `index` 변수에  "1번" 이라는 `String` 형을 넣으려고 하면, `BindException` 이 발생한다. `@ModelAttribute` 을 활용해서 특정 파라미터만을 받아올 수도 있다. 예를 들어 `{ country : "Korea" , capital : "Seoul" }` 형태의 데이터를 전송했다고 하면, 컨트롤러에서 `@ModelAttribute("capital") String captial` 의 형태로 `capital` 변수에  `Seoul`  만 바인딩시켜서 받아올 수 있다.  ( [출처](https://mangkyu.tistory.com/72) )

- **@RequestBody** 와  **@ModelAttribute**의 차이 : **@RequestBody** 는 요청받은 데이터를 변환하는 것이기 때문에 `Setter` 함수가 없어도 값이 매핑되지만, **@ModelAttribute** 는 바인딩하는 어떤 데이터를 set 하는 `Setter` 함수가 없다면 매핑이 되지 않는다.  ( [출처](https://mangkyu.tistory.com/72) )

- **@Transactional** : 일반적으로 스프링에서 `Service Layer` 에서  `@Transactional` 을 사용하여 트랜잭션 처리를 한다. 데이터 조회만 일어나는 `select` 메소드에서는 `@Transactional` 을 활용하지 않지만, 값을 추가하거나 변경 또는 삭제하는 `insert` ,  `update`,  `delete`  메소드에서는 `@Transactional` 을 추가하여 트랜잭션을 설정한다.  ( [출처](https://mangkyu.tistory.com/50?category=761302) )

- **@Valid** :  JSR-303 표준 스펙으로, **제약 조건이 부여된 객체**에 대해 빈 검증기 ( `Bean Validator` ) 가 검증하도록 지시하는 어노테이션이다. 스프링에서는 `LocalValidatorFactoryBean`을 이용해 JSR 표준의 검증 기능을 사용할 수 있는데, `LocalValidatorFactoryBean`은 JSR-303의 검증 기능을 이용할 수 있도록 도와주는 일종의 어댑터이다. JSR 표준의 빈 검증 기술의 특징은 객체의 필드에 달린 제약조건 어노테이션을 참고해 검증을 편리하게 할 수 있다는 것이다. 만약 검증에 오류가 있다면 `MethodArgumentNotValidException` 예외가 발생하고, 디스패처 서블릿에 기본으로 등록된 예외 리졸버 ( `Exception Resolver` ) 인 `DefaultHandlerExceptionResolver` 에 의해 `400 BadRequest` 에러가 발생한다. ( [출처](https://mangkyu.tistory.com/174?category=761302) ) 
  - JSR 표준 스펙은 다양한 제약 조건 어노테이션을 제공한다. 대표적인 어노테이션은 아래와 같다.
    - `@NonNull` : 해당 값이 `null`이 아닌지 검증함
    - `@AssertTrue` : 해당 값이 `true`인지 검증함
    - `@Size` : 해당 값이 주어진 값 사이에 해당하는지 검증함 ( `String`, `Collection`, `Map`, `Array` 에도 적용 가능함 )
    - `@Min` : 해당 값이 주어진 값보다 작지 않은지 검증함
    - `@Max` : 해당 값이 주어진 값보다 크지 않은지 검증함
    - 더 많은 어노테이션을 보고 싶다면, [자바 공식 문서](https://javaee.github.io/javaee-spec/javadocs/javax/validation/constraints/package-summary.html)(Java 8 기준)를 참고하자.

- **@Validated** : 객체를 검증하는 방법은 경우에 따라 달라질 수 있다. 예를 들어 일반 사용자의 요청과 관리자의 요청을 보내는 경우에 같은 객체로 요청이 오지만 다른 방식의 검증이 필요할 수 있다. 이런 경우에는 검증에 사용할 제약 조건이 2가지로 나뉘어야 한다. JSR-303 에서는 이런 경우를 위해 제약 조건 **어노테이션에 조건이 적용될 검증 그룹을 지정하여 적용** 할 수 있도록 **@Validated** 를 제공한다.  ( [출처](https://mangkyu.tistory.com/174?category=761302) )



## Spring Cloud

- 스프링 클라우드는 개발자가 **분산 시스템**에서 **공통 패턴**을 빠르게 구축할 수 있도록 도구를 제공한다.  ( [출처](https://spring.io/projects/spring-cloud#overview) )

  > Spring Cloud provides tools for developers to quickly build some of the common patterns in distributed systems (e.g. configuration management, service discovery, circuit breakers, intelligent routing, micro-proxy, control bus, one-time tokens, global locks, leadership election, distributed sessions, cluster state).

- **스프링 클라우드의 특징**

  - **일반적인 유즈 케이스**에 대하여 우수한 경험을 제공하고, **확장성 있는 메커니즘**을 제공하는 데 중점을 둔다.   ( [출처](https://spring.io/projects/spring-cloud#overview) )

    > Spring Cloud focuses on providing good out of box experience for typical use cases and extensibility mechanism to cover others.

  - 분산/버전화된 구성  (  `Distributed/versioned configuration ` )

  - 서비스 등록 및 검색  (  `Service registration and discovery`  )

  - 라우팅  (  `Routing`  )

  - 서비스 간 호출  (  `Service-to-service calls ` )

  - 로드 밸런싱 ( 부하 분산 )  (  `Load balancing`  )

  - 서킷 브레이커  (  `Circuit Breakers`  )

  - 전역 잠금  (  `Global locks`  )

  - 클러스터 상태  (  `Leadership election and cluster state`  )

  - 분산 메시징  (  `Distributed messaging`  )

- **Feign** :  `Feign`은  **Java to HTTP 클라이언트 바인더** 이다.  `Retrofit`,  `JAXRS-2.0` 및 `WebSocket` 에서 영감을 얻었다. `Feign` 은  [`Denominator`](https://github.com/Netflix/Denominator) 를  `HTTP API` 에 균일하게 바인딩하는 것의 복잡성을 줄이고자 한다.  ( [출처](https://github.com/OpenFeign/feign) )

  > Feign is a Java to HTTP client binder inspired by [Retrofit](https://github.com/square/retrofit), [JAXRS-2.0](https://jax-rs-spec.java.net/nonav/2.0/apidocs/index.html), and [WebSocket](http://www.oracle.com/technetwork/articles/java/jsr356-1937161.html). Feign's first goal was reducing the complexity of binding [Denominator](https://github.com/Netflix/Denominator) uniformly to HTTP APIs regardless of [ReSTfulness](http://www.slideshare.net/adrianfcole/99problems). 

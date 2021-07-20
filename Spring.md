## Spring

- **스프링** : 스프링이란 IoC와 AOP를 지원하는 경량의 컨테이너 프레임워크이다. ( [출처](https://devlog-wjdrbs96.tistory.com/165?category=882236) )

- **Bean** ( 빈 ) : 스프링 IoC 컨테이너가 관리하는 객체 ( [출처](https://devlog-wjdrbs96.tistory.com/165) )

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

- **DTO** ( `Data Transfer Object` ) : **데이터 전송 객체** , **계층( `Layer` ) 간** 데이터 교환을 위해 사용하는 객체이다. 로직을 가지고 있지 않은 순수한 객체이며, `getter/setter` 메소드만을 갖는다. 쉽게, 데이터를 전송할 때 사용하는 바구니라고 생각하자. ( [출처](https://www.youtube.com/watch?v=J_Dr6R0Ov8E) )

  - `Entity`를 `DTO` 대신 사용할 수 있지 않을까?

    - 사용할 수는 있지만.. `View`에서 표현하는 속성 값들이 요청에 따라 계속 달라질 수 있는데, 그 때마다 `Entity`의 속성값을 변경하면 영속성 모델을 표현한 `Entity`의 순수성이 모호해지기 때문에 `Controller` 에서 쓸 `DTO`와 `Entity`  클래스는 분리하는 게 좋다.

    > **Entity** : 실제 DB의 테이블과 매핑되는 클래스로, `id`로 구분되며, 로직을 포함할 수 있다.

  - client( browser ) <- **dto** -> controller( web ) - service - repository ( dao ) <- **domain(entity)** -> DB

- **MockMvc** : 웹 애플리케이션을 애플리케이션 서버에 배포하지 않고도 스프링 MVC의 동작을 재현할 수 있는 클래스이다. ( [출처](https://itmore.tistory.com/entry/MockMvc-%EC%83%81%EC%84%B8%EC%84%A4%EB%AA%85) )

- **Dispatcher Servlet** : 모든 **요청을 한곳에서 받아서 필요한 처리**들을 한 뒤, **요청에 맞는 handler로 요청을 Dispatch**하고, 해당 **Handler의 실행 결과를 Http Response 형태로 만드는 역할**을 한다. ( [출처](https://galid1.tistory.com/525) )

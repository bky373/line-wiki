# Spring in Action

- **책 정보**

  - 저자 : 크레이그 월즈
  - 옮긴이 : 심채철
  - 출판사 : 제이펍
  - 출간일 : 2020년 05월 14일
  - 에디션 : 5th Edition
  - 관련링크 :  [실습용 소스 코드](https://github.com/Jpub/SpringInAction5)

- **1.1 스프링이란? (p4~6)**

  - 스프링은 **스프링 애플리케이션 컨텍스트**(Spring application context) (**컨테이너**) 를 제공하고, 이는 애플리케이션 컴포넌트들을 생성하고 관리한다. 그리고 **애플리케이션 컴포넌트** 또는 **빈**(bean)들은 스프링 애플리케이션 컨텍스트 내부에서 서로 연결되어 완전한 애플리케이션을 만든다.

  - 빈의 상호 연결은 **의존성 주입**(Dependency Injection, DI)이라고 알려진 패턴을 기반으로 수행된다. 즉, 애플리케이션 컴포넌트에서 의존(사용)하는 다른 빈의 생성과 관리를 개발자가 작업하는 대신 스프링이 컨테이너가 해주며, 이 개체(컨테이너)에서는 모든 컴포넌트를 생성, 관리하고 해당 컴포넌트를 필요로 하는 빈에 주입(연결)한다. 일반적으로 이 작업은 생성자 인자 또는 속성의 접근자 메서드를 통해 처리된다. 

  - 과거의 스프링 버전에서는 컴포넌트 및 다른 컴포넌트와의 관계를 나타내기 위해 하나 이상의 XML 파일을 사용하였다. 

  - 하지만 최신 버전의 스프링에서는 자바 기반의 구성(configuration)을 더 많이 사용한다. 자바 기반 구성 클래스의 예시는 다음과 같다.

    ```java
    @Configuration
    public class ServiceConfiguration {
        @Bean
        public InventoryService inventoryService() {
            return new InventoryService();
        }
        @Bean
        public ProductService productService() {
            return new ProductService(inventoryService());
        }
    }
    ```

  - 여기서 @Configuration 애노테이션은 각 빈이 스프링 애플리케이션 컨텍스트에 제공하는 구성 클래스라는 것을 스프링에게 알려준다. 위의 클래스의 메서드에는 @Bean 애노테이션이 지정되어 있는데, 이는 각 메서드의 **반환 객체가 애플리케이션 컨텍스트의 빈** 으로 추가되어야 함을 나타낸다. 
  - 참고로 XML 기반 구성이나 위의 자바 기반 구성은 스프링의 자동-구성을 할 수 없을 경우에 사용한다. 자동-구성은 스프링의 자동 연결(autowiring) 및 컴포넌트 검색(component scanning) 기술을 기반으로 한다. 컴포넌트 검색을 사용하여 스프링은 자동으로 애플리케이션의 **classpath에 지정된 컴포넌트를 찾은 후 스프링 애플리케이션 컨텍스트의 빈으로 생성** 할 수 있다. 또한, **자동 연결을 사용하여 의존 관계가 있는 컴포넌트를 자동으로 다른 빈에 주입(연결)** 한다.
  - 최근에는 **스프링 부트(Spring Boot)** 가 소개되면서 자동-구성 기능이 더욱 향상되었다. 스프링 부트는 **생산성 향상을 제공하는 스프링 프레임워크의 확장** 이며, 향상된 자동-구성 기능 덕분에 환경 변수 classpth를 기준으로 어떤 컴포넌트가 구성되고 연결되어야 하는지 알 수 있다.

- **1.2 스프링 애플리케이션 초기 설정하기 (p7)**

  - 스프링 Initializr(이니셜라이저)를 사용하면 애플리케이션의 초기 설정을 쉽게 할 수 있다. 이를 통해 애플리케이션의 **프로젝트 디렉터리 구조 생성** 과  **빌드 명세 정의**에 드는 시간을 확 줄일 수 있다.
    - 스프링 Initializr는 REST API를 사용하는 브라우저 기반의 웹 애플리케이션이며, 스프링 프로젝트의 구조를 생성해준다. 

- **1.2.1 STS를 사용해서 스프링 프로젝트 초기 설정하기 (p8~p12)**

  - STS는 이클립스를 기반으로 하는 스프링 IDE이다. 스프링 부트 대시보드 기능을 제공한다.
  - JAR(Java ARchive) 패키징은 클라우드를 염두에 둔 선택이다. WAR(Web application ARchive) 파일은 기존의 자바 애플리케이션 서버에 애플리케이션을 배포할 때는 적합하지만, 대부분의 클라우드 플랫폼에는 잘 맞지 않는다. 모든 자바 클라우드 플랫폼은 실행 가능한 JAR 파일을 사용한다. 스프링 Initializr에서도 JAR 패키징을 기본값으로 사용한다.
  - 스프링 Initializr에서 프로젝트에 추가할 의존성(dependency)을 지정할 수 있다(의존성에는 프로젝트의 애플리케이션에서 사용할 외부 라이브러리 모듈을 지정하며, 여러 컴포넌트 클래스가 포함된 jar 파일이 주로 사용된다).

- **도메인 객체에 애노테이션 추가하기(p104~)**

  - 특정 클래스를 JPA 개체(entity)로 선언하려면 반드시 @Entity 애노테이션을 ㅜ가해야 한다. 그리고 이것의 id 속성에는 반드시 @Id 를 지정하여 이 속성이 데이터베이스의 개체를 고유하게 식별한다는 것을 나타내야 한다. 

  - JPA에서는 개체가 인자 없는 생성자를 가져야 한다. 이를 위해 Lombok의 @NoArgsConstructor를 지정한다. 하지만 인자 없는 생성자의 사용을 원하지 않을 경우, access 속성을 AccessLevel.PRIVATE으로 설정하여 클래스 외부에서 사용하지 못하게 만들 수 있다. 그리고 초기화가 필요한 final 속성이 있다면 force 속성을 true로 설정한다. 이에 따라 Lombok이 자동 생성한 생성자에서 그 속성들을 null로 설정할 수 있다. 아래 예시처럼 작성하면 된다.

    ```java
    @NoArgsConstructor(access=AccessLevel.PRIVATE, force=true)
    ```

- **JPA 리퍼지터리 선언하기(p108~)**

  - JDBC 버전의 리퍼지터리에서는 리퍼지터리가 제공하는 메서드를 개발자가 명시적으로 선언한다.

  - 하지만 스프링 데이터에서는 그 대신 CrudRepository 인터페이스를 확장(extends)할 수 있다. 예를 들어 다음 코드에서는 새로운 인터페이스인 IngredientRepository를 보여준다. CrudRepository 인터페이스에는 데이터베이스의 CRUD(Create(생성), Read(읽기), Update(변경), Delete(삭제)) 연산을 위한 많은 메서드가 선언되어 있다. 

    ```java
    package tacos.data;
    
    import org.springframework.data.repository.CrudRepository;
    
    import tacos.Ingredient;
    
    public interface IngredientRepository extends CrudRepository<Ingredient, String> {
        
        // Iterable<Ingredient> findAll();
        // Ingredient findById(String id);
        // Ingredient save(Ingredient ingredient);
    }
    ```

  - CrudRepository 인터페이스의 첫 번째 매개변수는 리퍼지터리에 저장되는 개체 타입이고, 두 번째 매개변수는 개체 id 속성의 타입이다. 

  - 인터페이스와 이에 정의된 메소드의 구현을 위해 클래스 구현체를 작성해야 한다고 생각할 수 있다. 하지만 그럴 필요가 없다. 애플리케이션이 시작될 때 스프링 데이터 JPA는 각 인터페이스 구현체(클래스 등)를 자동으로 생성해주기 때문이다. 즉 스프링 데이터 JPA를 사용하면 리퍼지터리만 작성하면 된다. 그리고 JDBC 기반의 구현에서 하듯이 리퍼지터리를 필요한 곳에 주입해주면 된다. 

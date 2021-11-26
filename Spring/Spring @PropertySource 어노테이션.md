# @PropertySource 어노테이션

* 설명
    * `org.springframework.context.annotation` 에 속한 어노테이션이다.
    * 선언 방식으로 간단히 스프링 환경(Environment)에 PropertySource 를 제공한다.
    * @Configuration 클래스와 함께 사용한다(반드시 @Configuration이 아니라 @Component 등 다른 어노테이션과도 함께 사용할 수 있다).
* 사용 예시
    * 아래 예시대로 @PropertySource 를 사용하려면 `app.properties` 파일이 필요하다.
    * 해당 파일에는 키와 값의 쌍 `person.name=mark` 가 정의되어 있어야 한다.
    * @Configuration 클래스는 스프링 Environment의 PropertySource Set에 `app.properties` 를 제공한다(@PropertySource 를 통해 가능).
    * 최소한의 스프링 부트 프로젝트를 생성하기만 하면 아래와 같이 연습할 수 있다.
      ```java
      // AppConfig 클래스
      // @SpringBootApplication 이 적용된 클래스와 동일 레벨에서 아래 클래스를 정의하였다. 
      
      @Configuration
      @PropertySource("classpath:application.properties")
      public class AppConfig {
        
        @Autowired
        Environment env;
        
        @Bean
        public Person person() {
          Person person = new Person();
          person.setName(env.getProperty("person.name"));
          System.out.println("name = " + person.getName());
          return person;
        }
      }
      ```
        * Environment 객체는 @Autowired 되어 있고 TestBean 객체를 채울 때 사용된다.
        * `person.getName()`에 대한 호출은 위의 구성에 따라 `app.properties`를 참조하고 결과로 `mark` 를 반환한다.
        * 아직 Person 클래스가 정의되어 있지 않으므로 아래와 같이 정의하도록 하자.
        ```java
        public class Person {

          private String name;
        
          public Person() {
          }
        
          public String getName() {
            return name;
          }
        
          public void setName(String name) {
            this.name = name;
          }
        }
        ```
        * Person 클래스 까지 작성했다면 Spring Application 을 실행(run)하자.
        * 로그를 확인하면 다음처럼 `name = mark` 가 출력된 것을 확인할 수 있다.
        <img width="478" alt="image" src="https://user-images.githubusercontent.com/49539592/143594538-5bc97237-1867-4a44-bf53-98a8e9167224.png">

* ${...} 사용
  * @PropertySource 리소스 위치 내 모든 ${...} 자리 표시자는 환경에 이미 등록된 속성 소스 로 값이 채워진다. 다음 예시를 보자.
  ```java
    @Configuration
    @PropertySource("classpath:/${mydirectorypath:default/path}/application.properties")
    public class AppConfig {
    
        @Autowired
        Environment env;
        
        @Bean
        public Person testBean() {
            Person person = new Person();
            person.setName(env.getProperty("person.name"));
            System.out.println("name = " + person.getName());
            return person;
        }
    }
  ```
  * `mydirectorypath`가 이미 등록된 속성 소스 중 하나(시스템 속성 또는 환경 변수)라고 가정하면 자리 표시자는 해당 값으로 확인된다. 
  * 그렇지 않은 경우 콜론(:)으로 구분된 `default/path`가 기본값으로 사용된다. 기본값을 표시하는 것은 선택 사항이다.
  * 기본값이 지정되지 않고 속성을 확인할 수 없으면 IllegalArgumentException이 발생한다.
> [출처](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/PropertySource.html)

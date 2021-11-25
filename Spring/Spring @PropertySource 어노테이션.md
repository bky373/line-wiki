# @PropertySource 어노테이션

* 설명
    * `org.springframework.context.annotation` 에 속한다.
    * 선언적인 메커니즘으로 스프링 환경(Environment)에 PropertySource 를 편리하게 정의해준다.  
    * @Configuration 클래스와 함께 사용된다.
* 사용 예시
    * 아래 예시대로 @PropertySource 를 사용하려면 `app.properties` 파일이 필요하다.
    * 해당 파일에는 키와 값의 쌍 `testbean.name=myTestBean` 이 정의되어 있어야 한다.
    * @Configuration 클래스는 @PropertySource 를 통해 Environment 의 PropertySource 세트(Set)에 `app.properties` 를 제공한다.
      ```java
      @Configuration
      @PropertySource("classpath:/com/myco/app.properties")
      public class AppConfig {
        
        @Autowired
        Environment env;
      
        @Bean
        public TestBean testBean() {
          TestBean testBean = new TestBean();
          testBean.setName(env.getProperty("testbean.name"));
          return testBean;
        }
      }
      ```
        * Environment 객체는 @Autowired 되어 있고 TestBean 객체를 채울 때 사용된다.
        * 위의 구성에 따르면 `testBean.getName()`에 대한 호출은 `app.properties`에 정의된 내용에 따라 "myTestBean" 을 반환한다.
    
  > [출처](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/PropertySource.html)

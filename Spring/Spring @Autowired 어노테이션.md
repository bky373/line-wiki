# @Autowired 어노테이션

## 1. 개요

* 스프링 프레임워크는 버전 2.5 부터 어노테이션 기반(annotations-driven)의 종속성 주입을 도입하였다.
* 이 기능의 주요 어노테이션으로 @Annotation 이 있고, 스프링은 이 어노테이션을 통해 빈을 주입하여 여러 빈 사이의 협력을 지원한다.
* 이번 문서에서는 Autowiring(자동연결)을 활성화하는 방법과 빈을 autowire(자동연결)하는 다양한 방법을 살펴볼 것이다.
* 그런 다음 @Qualifier 주석을 사용하여 빈 충돌을 해결하는 방법과 그밖의 잠재적인 예외 시나리오에 대해 살펴보려 한다.



### 2. @Autowired 어노테이션 활성화

* 스프링 프레임워크는 자동 의존성 주입을 지원한다. 즉, 스프링 구성 파일에서 모든 Bean 종속성을 선언함으로써 스프링 컨테이너는 협업하는 Bean들 사이의 관계를 자동연결할 수 있다. 이것을 `Spring bean autowiring` 이라고 한다.
* 자바 기반의 구성에서 스프링 Bean 을 인식할 수 있도록 하려면 다음과 같이 구성 클래스를 정의하자(@ComponentScan은 스프링에서 사용될 구성 요소들을 스캔하기 위해 사용된다. 여기에서는 역할을 간단히만 이해하고 아래 @SpringBootApplication 어노테이션을 살펴보자).
  ```java
  @Configuration
  @ComponentScan("com.autowire.sample")
  public class AppConfig {}
  ```
* 스프링 부트의 경우, @SpringBootApplication 어노테이션 하나로, @Configuration, @EnableAutoConfiguration, @ComponentScan 어노테이션을 모두 사용한 효과를 낼 수 있다.
* 애플리케이션의 메인 클래스에서 @SpringBootApplication 어노테이션을 사용하자.
  ```java
  @SpringBootApplication
  class AutowiredApplication {
    public static void main(String[] args) {
      SpringApplication.run(AutowiredApplication.class, args);
    }
  }
  ```
* 결과적으로 이 스프링 애플리케이션을 실행(run)하면 현재 패키지와 하위 패키지의 구성 요소들(components)을 자동으로 스캔한다.
* 따라서 스프링 애플리케이션 컨텍스트에 구성 요소들을 등록하고 @Autowired 를 사용하여 빈을 주입할 수 있다.



## 3. @Autowired 사용

* 어노테이션 주입을 활성화한 후, 속성(properties), 세터 및 생성자에서 자동연결(autowiring)을 사용할 수 있다.

### 3.1. 속성에서의 @Autowired

* 속성에 @Autowired를 사용하는 방법을 살펴보자. 이렇게 하면 getter와 setter가 필요하지 않다.
* 먼저 autowiredFormatter 빈을 정의하자.
  ```java
  @Component("autowiredFormatter")
  public class AutowiredFormatter {
    public String format() {
      return "autowired formatter";
    }
  }
  ```
* 그 다음 필드 정의에서 @Autowired를 사용하여 이 빈을 AutowiredService 빈에 주입한다.
  ```java
  @Component
  public class AutowiredService {
  
    @Autowired
    private AutowiredFormatter autowiredFormatter;
  }
  ```
* 결과적으로 스프링은 AutowiredService 가 생성될 때 autowiredFormatter 를 주입한다.

### 3.2. 세터에서의 @Autowired

* 이제 setter 메서드에 @Autowired를 추가해보자.
* 다음 예제에서는 AutowiredService가 생성될 때 autowiredFormatter의 인스턴스와 함께 setter 메서드가 호출된다.
  ```java
  public class AutowiredService {
      private AutowiredFormatter autowiredFormatter;
  
      @Autowired
      public void setAutowiredFormatter(AutowiredFormatter autowiredFormatter) {
        this.autowiredFormatter = autowiredFormatter;
      }
  }
  ```
  
### 3.3. 생성자에서의 @Autowired

* 아래 예시에서는 스프링에 의해 autowiredFormatter의 인스턴스가 AutowiredService의 생성자 인수로 주입된다.
  ```java
  public class AutowiredService {
      private AutowiredFormatter autowiredFormatter;
  
      @Autowired
      public AutowiredService(AutowiredFormatter autowiredFormatter) {
        this.autowiredFormatter = autowiredFormatter;
      }
  }
  ```



## 4. @Autowired 선택적 종속성

* 빈이 구성될 때 @Autowired 종속성을 사용할 수 있어야 한다. 만약 스프링이 연결을 위해 빈을 찾지 못하면 아래와 같은 예외가 발생한다.
  ```java
  Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: 
  No qualifying bean of type [com.autowire.sample.FooDAO] found for dependency:
  expected at least 1 bean which qualifies as autowire candidate for this dependency.
  Dependency annotations:
  {@org.springframework.beans.factory.annotation.Autowired(required=true)}
  ```
* 이 문제를 해결하려면 required type 정의를 포함하여 빈을 선언해야 한다.
  ```java
  public class AutowiredService {
    @Autowired(required = false)
    private AutowiredDAO dataAccessor;
  }
  ```
  


## 5. Autowire 명확성

* 기본적으로 스프링은 타입별로 @Autowired 항목을 해결한다. 만약 컨테이너에서 동일한 타입의 빈을 두 개 이상 사용할 경우, 프레임워크는 치명적인 예외를 던질 수 있다.
* 이 충돌을 해결하려면 주입하려는 빈을 명시적으로 스프링에게 알려주어야 한다.

### 5.1. @Qualifier 에 의한 자동연결

* @Qualifier 를 사용하여 필요한 빈을 명시적으로 표시하는 방법을 알아보자.
* 먼저 Formatter 타입의 빈 2개를 정의하자.
  ```java
  @Component("fooFormatter")
  public class FooFormatter implements Formatter {
    public String format() {
      return "foo";
    }
  }
  ```
  ```java
  @Component("barFormatter")
  public class BarFormatter implements Formatter {
    public String format() {
      return "bar";
    }
  }
  ```
* 이제 FooService 클래스에서 Formatter 빈을 주입한다고 해보자.
  ```java
  public class FooService {
    @Autowired
    private Formatter formatter;
  }
  ```
* 위 예제에서 컨테이너는 두 가지 Fomatter 타입의 구현체를 가지고 있다. 
* 스프링의 자동연결이 타입 기반으로 이루어지기 때문에 결국 FooService를 생성할 때 스프링은 NoUniqueBeanDefinitionException 예외를 던지게 된다.
  ```java
  Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: 
  No qualifying bean of type [com.autowire.sample.Formatter] is defined:
  expected single matching bean but found 2: barFormatter,fooFormatter
  ```
* 하지만 다음과 같이 @Quailfier 주석으로 구현체를 명시해준다면 위의 문제를 해결할 수 있다.
  ```java
  public class FooService {
    @Autowired
    @Qualifier("fooFormatter")
    private Formatter formatter;
  }
  ```
* 이처럼 동일한 유형의 빈이 여러 개 있는 경우 모호성을 피하기 위해 @Qualifier를 사용하는 것이 좋다.
* @Qualifier 주석의 값은 FooFormatter 구현의 @Component 주석에 선언된 이름과 일치한다(여기에서는 fooFormatter가 그러하다).

### 5.2. 커스텀 Qualifier에 의한 자동연결

* 스프링은 커스텀 @Qualifier 어노테이션 역시 지원한다. 커스텀 어노테이션을 생성하려면 아래와 같이 @Qualifier 어노테이션을 정의할 수 있어야 한다.
  ```java
  @Qualifier
  @Target({
    ElementType.FIELD, ElementType.METHOD, ElementType.TYPE, ElemnetType.PARAMETER})
  @Retention(RetentionPolicy.RUNTIME)
  public @interface FormatterType {
    String value();
  }
  ```
* 그런 다음 다양한 구현 내에서 FormatterType을 사용하여 커스텀한 값을 지정할 수 있다.
  ```java
  @FormatterType("Foo")
  @Component
  public class FooFormatter implements Formatter {
    public String format() {
      return "foo";
    }
  }
  ```
  ```java
  @FormatterType("Bar")
  @Component
  public class BarFormatter implements Formatter {
    public String format() {
      return "bar";
    }
  }
  ```
  ```java
  @Component
  public class FooService {
    @Autowired
    @FormatterType("Foo")
    private Formatter formatter;
  }
  ```
* @Target 메타 주석에 지정된 값은 한정자(qualifier)를 적용할 위치를 제한한다. 이 예에서는 필드, 메서드, 유형 및 매개변수를 사용한다.

### 5.3. 이름에 의한 자동연결

* 스프링은 빈의 이름을 기본 한정자(qualifier) 값으로 사용한다. 즉, 스프링은 컨테이너에서 autowire하는 속성을 찾을 때 이름이 정확히 일치하는 bean을 찾는다.
* 따라서 아래 예제에서 스프링은 fooFormatter 라는 속성 이름과 FooFormatter 구현체를 하나로 본다. 그래서 FooService를 구성할 때 해당 구현체를 주입한다.
  ```java
  public class FooService {
    @Autowired
    private Formatter fooFormatter;
  }
  ```

> [출처](https://www.baeldung.com/spring-autowire)

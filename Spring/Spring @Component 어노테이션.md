# @Compoenet 어노테이션

## 1. 개요

* 스프링의 @Component 어노테이션과 그와 관련된 영역을 포괄적으로 살펴본다. 

[comment]: <> (* 또한 일부 스프링 코어 기능와 통합하는 데 사용되는 방법들과 몇몇 이점들에 대해서도 살펴보려 한다.)



## 2. 스프링 애플리케이션 컨텍스트(Spring ApplicationContext)

* @Component의 값을 이해하기 전에 먼저 Spring ApplicationContext에 대해 알아야 한다.
* Spring ApplicationContext는 스프링에 의해 자동으로 관리되고 식별된 객체들의 인스턴스를 보유하는 곳이다. 이러한 인스턴스들을 빈(bean)이라 하고, 이러한 **빈 관리 와 종속성 주입** 은 스프링의 주요 기능에 속해 있다.
* 스프링은 Inversion of Control 원리를 사용하여 애플리케이션에서 빈 인스턴스를 수집하고 적절한 때에 이를 사용한다. 사용자는 객체를 따로 설정(setup)하거나 인스턴스화 할 필요 없이 스프링에 빈 종속성을 표시할 수 있다.
* 특히 @Autowired와 같은 어노테이션을 통해 애플리케이션에 스프링이 관리하는 빈을 주입할 수 있다. 이 기능은 스프링을 활용하여 확장 가능한 코드를 생성하는 데 매우 중요한 부분이라고 할 수 있다.
* 그렇다면 스프링에 빈을 알리는 방법은 무엇일까?
* 스프링이 빈을 감지할 수 있게 하려면 클래스에서 스테레오타입 어노테이션(stereotype annotations)을 사용하여 스프링의 자동 빈 감지 기능이 동작하게 해야 한다(아래 참고).



## 3. @Component

* @Component는 스프링이 사용자가 정의한 빈을 자동으로 감지할 수 있도록 하는 어노테이션이다.
* 즉, 다른 어떤 명시적 코드의 작성 없이 스프링은 다음을 수행한다.
  * @Component 주석이 달린 클래스를 찾기 위해 애플리케이션을 스캔한다.
  * @Component 이 달린 클래스를 인스턴스화 하고 해당 클래스에 지정된 종속성을 주입한다.
  * 필요로 하는 곳이 있을 때 @Component 이 달린 클래스를 주입한다.
* 그러나 대부분의 개발자는 이 기능을 제공하기 위해 보다 전문적인 스테레오타입 주석을 사용하는 것을 선호한다(다음 참고).

### 3.1. 스프링 스테레오타입 주석

* 스프링은 @Controller, @Service 및 @Repository와 같은 몇 가지 특수 스테레오타입 주석을 제공해오고 있다. 
* 이들 모두는 @Component와 동일한 기능을 제공하는데, 각각 @Component를 메타 주석으로 갖기 때문이다. 
* 단순히 생각하면 각각은 특수한 용도 하에 사용되는 @Component 별칭이라고 할 수 있다.
* 다음은 스프링 부트 프로젝트에서 각각의 어노테이션을 사용하는 예시다.
  ```java
    @Controller
    public class ControllerExample {
    }
    
    @Service
    public class ServiceExample {
    }
    
    @Repository
    public class RepositoryExample {
    }
    
    @Component
    public class ComponentExample {
    }
  ```
  ```java
    @Target({ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Component
    public @interface CustomComponent {
    }
    
    @CustomComponent
    public class CustomComponentExample {
    }
  ```
* 각 어노테이션이 스프링에 의해 자동 감지되고 ApplicationContext에 추가된다는 것을 증명하기 위해 다음과 같이 테스트 코드를 짤 수 있다.
  ```java
    @SpringBootTest
    @ExtendWith(SpringExtension.class)
    public class ComponentUnitTest {
    
        @Autowired
        private ApplicationContext applicationContext;
    
        @Test
        public void givenInScopeComponents_whenSearchingInApplicationContext_thenFindThem() {
            assertNotNull(applicationContext.getBean(ControllerExample.class));
            assertNotNull(applicationContext.getBean(ServiceExample.class));
            assertNotNull(applicationContext.getBean(RepositoryExample.class));
            assertNotNull(applicationContext.getBean(ComponentExample.class));
            assertNotNull(applicationContext.getBean(CustomComponentExample.class));
        }
    }
  ```
  
### 3.2. @ComponentScan

* 스프링은 @ComponentScan 을 사용하여 @Component가 달린 빈들을 모두 ApplicationContext로 수집한다(사실 @Component는 @CompentScan으로 수집되지 못하면 아무 일도 할 수 없다).
* 스프링 부트 애플리케이션을 사용하는 경우 @SpringBootApplication 어노테이션이 @ComponentScan을 포함하고 있다.
* @SpringBootApplication 클래스가 프로젝트의 루트에 있는 한, 스프링은 정의한 모든 @Component를 스캔한다고 보면 된다.
* 그러나 만약 @SpringBootApplication 클래스가 프로젝트의 루트에 있을 수 없거나, 외부 소스를 스캔하려는 경우 (classpath에 존재하는) 패키지를 찾도록 @ComponentScan을 명시적으로 구성할 수도 있다.
* 먼저, 범위를 벗어난 @Component 빈을 정의하자.
  ```java
  package com.component.outofscope;

  @Component
  public class OutOfScopeExample {
  }
  ```
* 다음으로 @ComponentScan 주석에 명시적인 경로를 포함하자.
  ```java
  package com.component.inscope;

  @SpringBootApplication
  @ComponentScan({"com.component.inscope", "com.component.outofscope"})
  public class ComponentApplication {
    //public static void main(String[] args) {...}
  }
  ```
* 범위 밖의 빈의 존재 여부를 테스트하는 코드는 다음과 같다.
  ```java
  @Test
  public void givenOutOfScopeComponent_whenSearchingInApplicationContext_thenFindIt() {
    assertNotNull(applicationContext.getBean(OutOfScopeExample.class));
  }
  ```
  
### 3.3. @Component 제한 사항

* 특정 객체가 스프링 관리 빈이 되기를 원하지만 @Component를 사용할 수 없는 경우가 있다.
* 예를 들어 프로젝트 외부의 패키지에 @Component 주석이 달린 객체를 정의해보자.
  ```java
  package com.baeldung.component.outsidescope;

  @Component
  public class OutsideScopeExample {
  }
  ```
* 다음은 ApplicationContext에 외부 구성 요소가 포함되어 있지 않음을 증명하는 테스트 코드다.
  ```java
  @Test
  public void givenOutsideScopeComponent_whenSearchingInApplicationContext_thenFail() {
    assertThrows(NoSuchBeanDefinitionException.class, () -> applicationContext.getBean(OutsideScopeExample.class));
  }
  ```
* 타사 소스 코드에 접근할 수 없으며 @Component 주석을 추가할 수 없는 경우도 있다. 
* 또는 실행 중인 환경에 따라 하나의 빈 구현을 다른 빈 구현보다 조건부로 사용하고 싶을 수도 있다. 대부분의 경우 자동 감지로 충분하지만 그렇지 않은 경우 @Bean을 사용할 수 있다.



## 4. @Component vs @Bean

* @Bean은 스프링이 런타임에 빈을 수집하는 데 사용하는 주석이기도 하지만 클래스 수준에서는 사용되지 않는다. **대신 메소드의 결과를 스프링 빈으로 저장할 수 있도록 메소드 위에 @Bean 주석을 달아준다.**
* 먼저 주석이 없는 POJO를 만들자.
  ```java
  public class BeanExample {
  }
  ```
* @Configuration 주석이 달린 클래스 내부에서 빈 생성 메서드를 만들 수 있다.
 ```java
  @Bean
  public BeanExample beanExample() {
      return new BeanExample();
  }
  ```
* BeanExample은 로컬 클래스 이거나 외부 클래스일 수 있다. 하지만 인스턴스를 반환하기만 하면 되기 때문에 무엇인지는 크게 중요하지 않다.
* 그런 다음 스프링이 빈을 선택했는지 확인하는 테스트를 작성할 수 있다.
  ```java
  @Test
  public void givenBeanComponent_whenSearchingInApplicationContext_thenFindIt() {
    assertNotNull(applicationContext.getBean(BeanExample.class));
  }
  ```
  
* @Component와 @Bean은 차이점이 있기 때문에 다음 몇 가지 사항을 주의해야 한다.
  * **@Component는 클래스 수준의 주석이고 @Bean은 메서드 수준의 주석이다.** @Component는 클래스의 소스 코드를 편집할 수 있는 경우에만 사용할 수 있다. @Bean은 항상 사용할 수 있지만 더 장황(verbose)하다는 특징을 갖는다.
  * **@Component는 스프링의 자동 감지와 호환되어 인스턴스화 코드를 작성하지 않아도 되지만, @Bean은 사용자가 클래스 인스턴스화 코드를 직접 작성해야 한다.**
  * @Bean을 사용한다는 것은 클래스 정의와 빈 인스턴스화 작업의 분리를 의미한다. 이는 사용자가 타사의 클래스를 스프링 빈으로 만들 수 있는 이유이기도 하다. 또한 이러한 점 덕분에 빈은 여러 인스턴스 옵션을 결정하는 논리를 도입할 수 있다.
  
> [출처](https://www.baeldung.com/spring-component-annotation)

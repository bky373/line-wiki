# @Qualifier 어노테이션

## 1. 개요

* 이번 문서에서 @Qualifier 주석이 사용자에게 어떤 도움을 주고, 어떤 문제를 해결하는지, 또 어떻게 사용하는지 살펴보려 한다.
* 또한 @Primary 주석과 이름으로 자동연결(autowiring by name)하는 것과 어떻게 다른지도 살펴볼 것이다.



## 2. 명확한 Autowiring 의 필요성

* @Autowired 주석은 스프링에서 의존성을 명시적으로 주입하기 위해 사용된다. 유용하기는 하지만 이 주석만으로는 스프링이 주입할 빈을 찾고 이해하기에 충분하지 않을 때가 있다.
* **기본적으로 스프링은 타입 별로 자동연결된 항목을 해결한다.**
* 만약 컨테이너에 2개 이상의 동일한 타입 Bean이 등록되어 있다면 프레임워크는 NoUniqueBeanDefinitionException을 던질 것이다.
* 다음 예시는 위의 상황을 코드로 보여준다.
  ```java
    @Component("fooFormatter")
    public class FooFormatter implements Formatter {
    
        public String format() {
            return "foo";
        }
    }
    
    @Component("barFormatter")
    public class BarFormatter implements Formatter {
    
        public String format() {
            return "bar";
        }
    }
    
    @Component
    public class FooService {
    
        @Autowired
        private Formatter formatter;
    }
  ```
* 컨텍스트에 FooService를 로드하려고 하면 스프링 프레임워크는 NoUniqueBeanDefinitionException을 throw한다. 
* 이는 스프링이 어떤 빈을 주입할지 모르기 때문이다. @Qualifier 주석은 이 문제를 해결하는 방법 중 하나다.



## 3. @Qualifier 어노테이션
> qualifier 는 명사로 `수식어구(형용사 및 부사) 또는 예선 통과자` 라는 사전적 의미를 가지고 있다.

* @Qualifier를 사용하면 어떤 빈을 주입해야 하는지 명시함으로써 위의 문제를 해결할 수 있다. 사용 예시는 다음과 같다.
  ```java
    public class FooService {
      
      @Autowired
      @Qualifier("fooFormatter")
      private Formatter formatter;
    }
  ```
* 사용할 때 주의할 점은 사용할 한정자(qualifier)의 이름이 @Component 주석에 선언된 이름이라는 점이다.
* 동일한 효과를 얻기 위해 @Component 주석에 이름을 지정하는 대신 Formatter 구현 클래스에서 @Qualifier 주석을 사용할 수도 있다.
  ```java
    @Component
    @Qualifier("fooFormatter")
    public class FooFormatter implements Formatter {
      //...
    }
    
    @Component
    @Qualifier("barFormatter")
    public class BarFormatter implements Formatter {
      //...
    }
  ```
  


## 4. @Qualifier vs @Primary

* 위에서 본 @Qualifier처럼 모호한 종속성 주입 문제를 해결하기 위해 @Primary라는 주석을 사용할 수도 있다.
* 이 주석은 **동일한 유형의 Bean이 여러 개 있는 경우 기본 설정을 정의한다.** 따라서 달리 명시되지 않는 한 @Primary 주석과 관련된 빈이 사용된다. 다음 예시를 보자.
  ```java
    @Configuration
    public class Config {
      
      @Bean
      public Employee markEmployee() {
        return new Employee("Mark");
      }
  
      @Bean
      @Primary
      public Employee davidEmployee() {
        return new Employee("David");
      }
    }
  ```
* 이 예에서 두 메서드는 동일한 Employee 타입을 반환한다. 하지만 종속성 주입시 스프링이 먼저 사용할 bean은 davidEmployee 메소드에 의해 반환되는 빈이다. 이 메서드에는 @Primary 주석이 포함되어 있기 때문이다. 이 주석은 기본적으로 특정 유형의 빈을 주입해야 할 때 유용하다.
* 어떤 주입 지점에서 다른 빈이 필요한 경우에는 @Qualifier으로 구체적으로 표시해주어야 한다. 예를 들어, @Qualifier 주석을 사용하여 markEmployee 메소드에서 반환된 빈을 사용하도록 지정할 수 있다.
* **@Qualifier와 @Primary 주석이 모두 있는 경우에는 @Qualifier 주석이 우선한다. 기본적으로 @Primary는 기본값을 정의하고 @Qualifier는 매우 구체적인 값을 정의한다고 보면 된다.**



## 5. @Qualifier vs 이름에 의한 자동연결(Autowiring by Name)

* Autowiring할 때 여러 bean 사이를 결정하는 또 다른 방법은 주입할 필드의 이름을 사용하는 것이다. 이것은 스프링에게 다른 힌트가 주어지지 않을 때 사용하는 기본 설정이다. 초기 예제를 다시 살펴보자.
  ```java
  public class FooService {
     
    @Autowired
    private Formatter fooFormatter; // 위의 예제에서는 필드 이름이 formatter 였다.
  }
  ```
* 이 경우 필드 이름(`fooFormatter`)이 @Component 주석에서 사용한 값(`@Component("fooFormatter")`)과 일치하기 때문에 스프링은 주입할 빈이 FooFormatter 임을 이해할 수 있게 된다.

> [출처](https://www.baeldung.com/spring-qualifier-annotation)

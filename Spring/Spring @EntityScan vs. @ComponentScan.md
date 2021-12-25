# Spring @EntityScan vs. @ComponentScan

## 1. 소개
* 먼저 이번 문서에서 사용할 용어를 명확히 하겠습니다.
    * **구성 요소(components)** 는 `@Controller`, `@Service`, `@Repository`, `@Component`, `@Bean` 등의 어노테이션이 있는 클래스를 의미합니다.
    * **엔티티(entities)** 는 `@Entity` 어노테이션으로 표시된 클래스를 의미합니다.
* 스프링 애플리케이션을 사용할 때 종종 **엔티티 클래스를 포함하는 특정 패키지 목록** 을 지정해야 할 때가 있습니다. 
* 또 다른 경우로, 초기화할 스프링 **빈의 특정 목록** 을 지정해야 할 때가 있습니다. 
* 이런 경우를 위해 `@EntityScan` 또는 `@ComponentScan` 어노테이션을 사용할 수 있습니다.
* 이번 문서에서는 `@EntityScan`과 `@ComponentScan`의 사용법과 차이점에 대해 살펴봅니다.


## 2. @EntityScan 어노테이션
* 스프링 애플리케이션을 작성할 때 일반적으로 엔티티 클래스를 사용합니다. 엔티티 클래스는 `@Entity` 어노테이션을 클래스 위에 붙여 표현합니다.
* 엔티티 클래스의 위치를 정할 때 두 가지 방법을 고려할 수 있습니다.
  * 애플리케이션의 메인(main) 패키지 또는 하위 패키지 아래에 위치 시킨다.
  * 완전히 다른 루트 패키지 아래에 위치 시키낟.
* 첫 번째 시나리오에서는 `@EnableAutoConfiguration` 을 사용하여 스프링이 애플리케이션 컨텍스트를 자동 구성하도록 할 수 있습니다.
* 두 번째 시나리오에서는 해당 패키지에 대한 정보를 애플리케이션에 제공하기 위해 `@EntityScan` 을 사용합니다.
* `@EntityScan` 어노테이션은 엔티티 클래스가 애플리케이션 메인 패키지 또는 해당 하위 패키지에 배치되지 않은 경우 사용됩니다. 
* `@EntityScan` 어노테이션 내의 메인 구성(configuration) 클래스에서 패키지 또는 패키지 목록을 선언합니다. 이것으로 스프링에게 애플리케이션에서 사용되는 엔티티의 위치 정보를 알려줄 수 있습니다.
    ```java
      @Configuration
      @EntityScan("com.bobo.demopackage")
      public class EntityScanDemo {
        // ...
      }
    ```
* 단, **`@EntityScan` 을 사용하면 엔티티를 스캔하는 스프링 부트 자동 구성(auto-configuration)이 비활성화된다는 점을 알아야 합니다.**


## 3. @ComponentScan 어노테이션
* `@EntityScan` 으로 특정 엔티티(entities)를 찾는 것과 유사하게, `@ComponentScan` 어노테이션으로 스프링이 특정 빈 클래스 세트만 찾도록 할 수 있습니다. 
* `@ComponentScan` 어노테이션은 **스프링이 초기화하기 원하는 빈 클래스의 특정 위치를 가리킬 것입니다.**
* 이 어노테이션은 매개변수를 포함하거나 포함하지 않고 사용할 수 있습니다. 
* 매개변수가 있으면 스프링은 지정된 위치의 패키지를 스캔합니다.
* 매개변수가 없으면 스프링은 현재 패키지와 하위 패키지를 스캔합니다. 
* 매개변수를 조금 더 살펴보겠습니다.
    * `basePackages` 매개변수를 사용하면 스캔하고 싶은 패키지의 목록을 전달할 수 있습니다. 
    * `basePackageClasses` 매개변수를 사용하면 스캔하고 싶은 패키지의 클래스의 이름을 지정할 수 있습니다(지정한 클래스의 패키지를 탐색 시작 위치로 지정합니다).
* 다음으로 `@ComponentScan` 어노테이션 사용의 예를 보겠습니다.
  ```java
    @Configuration
    @ComponentScan(
      basePackages = {"com.bobo.demopackage"},
      basePackageClasses = DemoBean.class)
    public class ComponentScanExample {
      // ...
    }
  ```

## 4. @EntityScan vs. @ComponentScan
* 두 어노테이션은 스프링 애플리케이션 구성에 기여한다는 점에서 유사해 보입니다.
* 하지만 둘의 목적은 완전히 다릅니다. 
* `@EntityScan` 은 스캔하고 싶은 엔티티 클래스의 패키지를 지정하는 데 사용합니다. 
* 반면에 `@ComponentScan` 은 스캔하고 싶은 스프링 빈의 패키지를 지정하는 데 사용합니다.


## 참고 자료
* [Spring @EntityScan vs. @ComponentScan](https://www.baeldung.com/spring-entityscan-vs-componentscan)

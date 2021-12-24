# Spring Bean Annotations

## 1. 개요
* 스프링 컨테이너에서 빈을 구성하는 방법에는 여러 가지가 있습니다. 
* XML 구성을 사용하여 빈을 선언할 수도 있고, 구성(configuration) 클래스에서 `@Bean` 어노테이션을 사용하여 빈을 선언할 수도 있습니다.
* 이번 문서에서는 **일반적으로 가장 많이 사용되는 스프링 빈 어노테이션들** 에 대해 알아봅니다.


## 2. Component Scanning
* **컴포넌트 스캐닝이 활성화(enable)되면**, 스프링은 자동으로 패키지 내에 선언된 빈을 스캔할 수 있습니다.
* 이때 `@ComponentScan` 은 **어노테이션 구성과 함께 클래스를 스캔할 패키지 정보를 구성합니다.** 
* `basePackages` 또는 `value` 인수 중 하나를 사용하여 기본(base) 패키지 이름을 직접 지정할 수 있습니다(`value` 는 `basePackages` 의 별칭입니다).
    ```java
        @Configuration
        @ComponentScan(basePackages = "com.example.annotations")
        class VehicleFactoryConfig {}
    ```
* 또한 `basePackageClasses` 인수를 사용하여 기본 패키지의 클래스를 가리킬 수 있습니다.
    ```java
        @Configuration
        @ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
        class VehicleFactoryConfig {}
    ```
* 두 인수 모두 배열 타입이기 때문에 여러 개의 패키지를 제공할 수 있습니다.
* 인수를 따로 지정하지 않으면 `@ComponentScan` 어노테이션 클래스가 위치해 있는 패키지에서 스캔이 발생합니다.
* `@ComponentScan` 은 자바 8의 어노테이션 반복 기능 덕분에 여러 번 작성하여 사용할 수 있습니다.
  ```java
    @Configuration
    @ComponentScan(basePackages = "com.example.annotations")
    @ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
    class VehicleFactoryConfig {}
  ```
* 또는 `@ComponentScans` 안에 여러 개의 `@ComponentScan` 구성을 지정할 수도 있습니다.
  ```java
    @Configuration
    @ComponentScans({
      @ComponentScan(basePackages = "com.example.annotations"),
      @ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
    })
    class VehicleFactoryConfig {}
  ```
* XML 구성으로 컴포넌트 스캔을 할 경우 다음과 같이 작성할 수 있습니다.
  ```xml
    <context:component-scan base-package="com.exampmle.annotations" />
  ```


## 3. @Component
* `@Component` 는 클래스 수준(class level) 어노테이션입니다. 
  ```java
    @Component
    class CarUtility {
      // ...
    }
  ```
* 컴포넌트 스캔 중에 **스프링 프레임워크는 `@Component` 어노테이션이 달린 클래스를 자동으로 감지합니다.**
* 기본적으로 이 클래스의 빈 인스턴스는 **소문자 이니셜** 을 가진 클래스 이름과 동일한 이름을 갖습니다. 하지만 어노테이션의 `value` 인수를 사용할 경우 다른 이름을 지정할 수 있습니다.
* `@Repository`, `@Service`, `@Configuration`, `@Controller` 는 모두 `@Component` 의 meta-annotation 이기 때문에 빈 인스턴스 네이밍 방식이 동일합니다. 
* **스프링은 컴포넌트 스캔 프로세스 중에 이름에 맞는 인스턴스를 자동으로 선택합니다.** 
* 따라서 위의 어노테이션을 적절히 사용하면 구성 클래스 안에 빈을 명시적으로 정의할 필요가 없기 때문에 구성을 더 작게 할 수 있습니다.


## 4. @Repository
* DAO 또는 Repository 클래스는 일반적으로 애플리케이션의 데이터베이스 액세스 계층을 나타내며 `@Repository` 어노테이션을 달아야 합니다.
  ```java
    @Repository
    class VehicleRepository {
      // ...
    }
  ```
* 어노테이션 사용의 이점 중 하나는 **영속성 예외 변환(persistence exception translation)이 자동으로 활성화되어 있다** 는 것입니다. 
* Hibernate와 같은 영속성 프레임워크를 사용할 때, `@Repository` 이 달린 클래스 안에서 발생하는 네이티브 예외들은 자동적으로 스프링의 `DataAccessExeption` 의 하위 클래스로 변환됩니다.
* 만약 **예외 번역을 따로 활성화하고 싶다면**, `PersistenceExceptionTranslationPostProcessor` 빈을 선언해야 합니다.
  ```java
    @Bean
    public PersistenceExceptionTranslationPostProcessor exceptionTranslation() {
      return new PersistenceExceptionTranslationPostProcessor();
    }
  ```
* 대부분의 경우 스프링은 위의 단계를 자동으로 수행합니다.
* 참고로, XML로 구성하는 방법은 다음과 같습니다.
  ```java
    <bean class="org.springframework.dao.annotation.PersistenceExceptionTranslationPostProcessor"/>
  ```
  

## 5. @Service
* 애플리케이션의 **비즈니스 로직** 은 일반적으로 서비스 계층 내에 상주하므로 `@Service` 어노테이션을 사용하여 클래스가 해당 계층에 속함을 나타냅니다.
  ```java
    @Service
    public class VehicleService {
      // ...    
    }
  ```

## 6. @Controller
* `@Controller` 는 (스프링 프레임워크에) 해당 클래스가 **스프링 MVC에서 컨트롤러** 역할을 한다고 알려주는 클래스 수준 어노테이션입니다.
  ```java
    @Controller
    public class VehicleController {
      // ...
    }
  ```
  
## 7. @Configuration
* 구성(Configuration) 클래스에는 `@Bean` 어노테이션이 달린 빈 정의 메서드(bean definition methods)가 포함될 수 있습니다.
  ```java
    @Configuration
    class VehicleFactoryConfig {
    
        @Bean
        Engine engine() {
            return new Engine();
        }
    
    }
  ```


## 8. 스테레오타입 어노테이션과 AOP
* 스프링 스테레오타입 어노테이션을 사용할 때 특정 스테레오타입을 가진 모든 클래스를 대상으로 포인트컷을 쉽게 생성할 수 있습니다.
* 예를 들어 DAO 계층에서 메서드의 실행 시간을 측정한다고 가정합니다. 
* `@Repository` 스테레오타입을 활용하여 다음의 aspect(AspectJ 어노테이션 사용)을 생성합니다.
  ```java
    @Aspect
    @Component
    public class PerformanceAspect {
    @Pointcut("within(@org.springframework.stereotype.Repository *)")
    public void repositoryClassMethods() {};
    
        @Around("repositoryClassMethods()")
        public Object measureMethodExecutionTime(ProceedingJoinPoint joinPoint) 
          throws Throwable {
            long start = System.nanoTime();
            Object returnValue = joinPoint.proceed();
            long end = System.nanoTime();
            String methodName = joinPoint.getSignature().getName();
            System.out.println(
              "Execution of " + methodName + " took " + 
              TimeUnit.NANOSECONDS.toMillis(end - start) + " ms");
            return returnValue;
        }
    }
  ```
* 이 예제에서는 `@Repository` 어노테이션이 달린 클래스의 모든 메서드와 일치하는 포인트컷을 만들었습니다. 
* `@Around` 어드바이스(advice)를 사용하여 해당 포인트컷을 타겟으로 하고 동작을 가로챈 메서드의 호출 실행 시간을 결정했습니다.
* 이 접근 방식을 사용하여 로깅, 성능 관리, 감사(audit) 또는 기타 동작을 각 애플리케이션 계층에 추가할 수 있습니다.


## 참고 자료
* [Spring Bean Annotations](https://www.baeldung.com/spring-bean-annotations)

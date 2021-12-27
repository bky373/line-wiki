# Spring Core Annotations

## 1. 개요
* `org.springframework.beans.factory.annotation`과 `org.springframework.context.annotation` 패키지의 어노테이션을 간단히 **스프링 핵심 어노테이션(Spring Core Annotations)** 이라 부릅니다.
* 이 어노테이션들을 사용하여 스프링 DI 엔진 기능을 활용할 수 있습니다.


## 2. DI 관련 어노테이션들

### 2.1. @Autowired
* `@Autowired` 어노테이션으로 스프링이 주입할 **종속성에 대해 간단히 표현** 할 수 있습니다.
* **생성자, 설정자(수정자) 또는 필드 주입** 을 표현할 때 이 어노테이션을 사용합니다.

#### 2.1.1. 생성자 주입
```java
class Car {
    Engine engine;

    @Autowired
    Car(Engine engine) {
        this.engine = engine;
    }
}
```
* 생성자 주입을 사용하는 경우 모든 생성자 인수가 필수적으로 필요합니다.
* 버전 4.3부터 생성자가 1개일 때 `@Autowired` 어노테이션을 생략할 수 있습니다.

#### 2.1.2. 설정자(수정자) 주입
```java
class Car {
  Engine engine;

  @Autowired
  void setEngine(Engine engine) {
    this.engine = engine;
  }
}
```

#### 2.1.3. 필드 주입
```java
class Car {
  @Autowired
  Engine engine;
}
```
* `@Autowired` 에는 기본값이 `true` 인 `required` 라는 boolean 인수가 있습니다.
* `required = true` 이면서 연결하기(wire)에 적합한 빈을 찾지 못하면 예외가 발생합니다. 
* 반면, 동일한 상화에서 `required = false` 인 경우에는 아무것도 연결되지 않습니다.
* 보다 자세한 내용은 [@Autowired](https://github.com/bky373/line-wiki/blob/main/Spring/Spring%20%40Autowired%20어노테이션.md) 또는 [생성자 주입](https://www.baeldung.com/constructor-injection-in-spring) 에서 확인할 수 있습니다.

### 2.2. @Bean
* `@Bean` 은 스프링 **빈을 인스턴스화하는 팩토리 메소드** 를 표현하기 위해 사용합니다.
    ```java
    @Bean
    Engine engine() {
        return new Engine();
    }
    ```
* **반환 타입의 객체가 필요해질 때 스프링은 해당 메서드를 호출합니다.**
* 이때 **빈의 이름은 팩토리 메소드와 동일한 이름을 갖습니다.** 
* 다른 이름을 지정하려면 이 어노테이션의 `name` 또는 `value` 인수를 사용하면 됩니다(`value` 는 `name` 의 별칭입니다).
    ```java
      @Bean("mainEngine")
      Engine engine() {
        return new Engine();
      }
    ```
* **`@Bean` 어노테이션이 달린 모든 메소드는 `@Configuration` 클래스에 있어야 합니다.**

### 2.3. @Qualifier
* 모호한 상황(예를 들어 동일한 타입의 두 개 이상의 빈이 생성된 상황)에서 **빈 ID 또는 빈의 이름을 명시적으로 제공** 하기 위해 `@Autowired` 와 함께 `@Qualifier` 를 사용합니다.
* 예를 들어 다음 두 빈은 동일한 인터페이스를 구현합니다.
    ```java
      class Bike implements Vehicle {}

      class Car implements Vehicle {}
    ```
* 여기에서 Car와 Bike 는 모두 `Vehicle` 이라는 빈의 정의와 일치합니다.
* 따라서 스프링이 `Vehicle` 빈을 주입해야 하는 경우, 특정 빈을 선택할 수 없는 모호한 상황이 발생합니다. 
* 이럴 때 `@Qualifier` 어노테이션을 사용하여 명시적으로 빈의 이름을 제공할 수 있습니다.

#### 2.3.1. 생성자 주입
```java
// Bike.java
@Component
@Qualifier("bike") // 생략 가능하나, @Qualifier 끼리 연결하여 사용하는 걸 추천합니다. 
class Bike implements Vehicle {}

// Biker.java
@Autowired
Biker(@Qualifier("bike") Vehicle vehicle) {
    this.vehicle = vehicle;
}
```

#### 2.3.2. 설정자(수정자) 주입
```java
// Biker.java
@Autowired
void setVehicle(@Qualifier("bike") Vehicle vehicle) {
    this.vehicle = vehicle;
}
```

#### 2.3.3. 필드 주입
```java
// Biker.java
@Autowired
@Qualifier("bike")
Vehicle vehicle;
```
* 보다 자세한 내용은 [여기](https://www.baeldung.com/spring-autowire) 에 있습니다.

### 2.4. @Required
* XML 구성일 때, 필수 속성의 종속성을 표시하기 위해 setter 메서드에 `@Required` 어노테이션을 달아줍니다.
    ```java
        // Bike.java
        @Required
        void setColor(String color) {
          this.color = color;
        }
    ```
    ```xml
        <bean class="com.example.annotations.Bike">
            <property name="color" value="green" />
        </bean>
    ```
* 필수 속성에 대한 설정 정보를 구성하지 않을 경우 `BeanInitializationException` 이 발생합니다.

### 2.5. @Value
* 빈의 속성에 값을 주입하기 위해 [@Value](https://www.baeldung.com/spring-value-annotation) 를 사용합니다. 
* 생성자, 설정자(수정자) 및 필드 주입시 사용됩니다.

#### 2.5.1. 생성자 주입
```java
Engine(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

#### 2.5.2. 설정자(수정자) 주입
```java
@Autowired
void setCylinderCount(@Value("8") int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```
또는
```java
@Value("8")
void setCylinderCount(int cylinderCount) {
    this.cylinderCount = cylinderCount;
}
```

#### 2.5.3. 필드 주입
```java
@Value("8")
int cylinderCount;
```
* 물론 위와 같이 하드코딩 값을 주입하는 것은 권장하지 않습니다. 
* 대신 `@Value` 의 placeholder 문자열을 사용하여 외부 소스(`.properties` 또는 `.yaml` 파일)에 정의된 값을 연결할 수 있습니다.
* 다음 예시를 보겠습니다.
```properties
// example.properties
engine.fuelType=petrol
```
* 위에서 정의한 `engine.fuelType` 값을 아래 `fuelType` 속성에 주입할 수 있습니다.
```java
@Value("${engine.fuelType}")
String fuelType;
```
* SpEL에서도 `@Value` 를 사용할 수 있습니다. 자세한 내용은 [@Value](https://github.com/bky373/line-wiki/blob/main/Spring/Spring%20%40Value%20어노테이션.md) 를 참조해주세요.


### 2.6. @DependsOn
* `@DependsOn` 어노테이션을 사용하면 스프링이 **해당 어노테이션을 붙인 빈보다 다른 빈을 먼저 초기화** 하도록 합니다. 
* 일반적으로 어노테이션의 동작은 빈 사이의 명시적인 종속성에 기반하여 자동으로 수행됩니다.
* 하지만 암시적(implicit)인 종속성일 경우(예를 들어, JDBC 드라이버 로딩 또는 정적 변수 초기화의 경우) 어노테이션을 붙여주기만 하면 됩니다.
* `@DependsOn` 은 종속성이 필요한 클래스 이름 위에 종속성 빈의 이름을 지정하여 사용할 수 있습니다. 
* 어노테이션의 `value` 인수에는 단일 값 또는 배열의 형태로 종속성 빈 이름을 넣을 수 있습니다.
  ```java
    @DependsOn("engine")
    class Car implements Vehicle {}
  ```
* `@Bean` 어노테이션으로 빈을 정의하는 경우 팩토리 메서드에 `@DependsOn` 어노테이션을 달아야 합니다.
  ```java
    @Bean
    @DependsOn("fuel")
    Engine engine() {
      return new Engine();
    }
  ```

### 2.7. @Lazy
* 스프링은 기본적으로 애플리케이션 컨텍스트의 시작 / 부트스트래핑시 모든 빈을 싱글톤 인스턴스로 생성합니다.
* 하지만 애플리케이션을 시작했을 때가 아니라, **빈에 대한 요청이 들어왔을 때** 빈을 생성해야 하는 경우도 있습니다.
* [@Lazy](https://www.baeldung.com/spring-lazy-annotation) 는 이와 같이 빈을 느리게 초기화하고 싶을 때 사용합니다. 
* 이 어노테이션은 정확히 어디에 두느냐에 따라 다르게 동작합니다.
  * `@Bean` 어노테이션이 달린 빈 팩토리 메서드: 빈을 생성하는 메서드 호출을 지연합니다.
  * `@Configuration` 클래스: 클래스 안의 모든 `@Bean` 메소드가 영향을 받습니다.
  * `@Configuration` 클래스가 아닌 `@Component` 클래스: 해당 빈이 느리게 초기화됩니다.
  * `@Autowired` 생성자, 설정자 또는 필드: (프록시를 통해) 종속성 자체를 느리게 로드합니다.
* 이 어노테이션은 `value` 라는 인수를 가지며 기본값이 `true` 입니다. 경우에 따라 디폴트 동작을 재정의하는 것이 유용합니다.
* 예를 들어 `@Lazy` 로 표시된 `@Configuration` 클래스에서 특정 `@Bean` 메서드를 즉시 로드하도록 구성할 수 있습니다.
  ```java
    @Configuration
    @Lazy
    class VehicleFactoryConfig {
    
        @Bean
        @Lazy(false)
        Engine engine() {
            return new Engine();
        }
    }
  ```
* 보다 자세한 내용은 [여기](https://www.baeldung.com/spring-lazy-annotation) 에 있습니다.

### 2.8. @Lookup
* `@Lookup` 어노테이션이 달린 메서드를 사용하면, 메서드 호출 시 해당 메서드의 반환 타입 인스턴스를 반환받을 수 있습니다.
* 보다 자세한 설명은 [여기](https://www.baeldung.com/spring-lookup) 에 있습니다.

### 2.9. @Primary
* 종종 같은 타입이지만 인스턴스가 다른 빈 여러 개를 정의할 수 있습니다. 
* 스프링은 같은 타입의 빈이 여러 개 존재하는 경우 우리가 필요로 하는 빈을 정확히 찾아주지 못합니다(`NoUniqueBeanDefinitionException` 이 발생합니다).
* 이 경우 모든 연결지점마다 `@Qualifier` 를 설정하여 문제를 해결할 수 있지만, `@Primary` 를 사용할 수도 있습니다.
* 특히 대부분의 경우, 특정 빈은 필요로 하지만 이와 같은 타입의 다른 빈은 필요로 하지 않을 때가 많습니다. 
* 이런 경우 `@Primary` 를 사용한다면, 문제를 간단히 해결할 수 있습니다. 방법은 **가장 자주 사용되는 빈을 `@Primary` 로 표시하기** 만 하면 됩니다.
  ```java
    @Component
    @Primary
    class Car implements Vehicle {}
    
    @Component
    class Bike implements Vehicle {}
    
    @Component
    class Driver {
  
      @Autowired
      Vehicle vehicle;
    }
    
    @Component
    class Biker {
  
      @Autowired
      @Qualifier("bike")
      Vehicle vehicle;
    }
  ```
* 앞의 예에서 Car는 기본(primary) 차량입니다. 
* 따라서 Driver 클래스에서 스프링은 vehicle 필드에 Car 빈을 주입합니다. 
* 반면, Biker 빈의 vehicle 필드에는 `@Qualifier` 가 사용되었기 때문에 Bike 빈을 주입합니다.

### 2.10. @Scope
* `@Scope` 를 사용하여 `@Component` 클래스 또는 `@Bean` 정의의 범위( [scope](https://www.baeldung.com/spring-bean-scopes) )를 정의할 수 있습니다. 
* 스코프는 싱글톤, 프로토타입, 요청, 세션, globalSession 또는 커스텀 정의 범위일 수 있습니다.
  ```java
    @Component
    @Scope("prototype")
    class Engine {}
  ```


## 참고 자료
* [Spring Core Annotations](https://www.baeldung.com/spring-core-annotations)

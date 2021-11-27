# @Value 어노테이션

## 1. 개요
  - (스프링이 관리하는) 빈의 필드에 값을 주입하거나, 생성자 / 메서드의 매개변수에 값을 주입하기 위해 사용된다.

## 2. 애플리케이션 설정

- @Value 어노테이션의 다양한 사용법을 익히기 위해 간단한 스프링 애플리케이션 Configuration 클래스를 구성한다.
    - 반드시 @Value 사용 위치가 Configuration 클래스일 필요는 없지만 여기에서는 Configuration 클래스를 사용한다.
- 다음으로 @Value 으로 주입할 값을 정의해야 하므로 **속성 파일(properties file)** 이 필요하다.

  ```properties
  // src/main/java/resources/application.properites
  
  value.from.file=Value got from the file
  priority=high
  listOfValues=A,B,C
  ```

- **속성 파일**을 작성했다면 Configuration 클래스 `@PropertySource` 에 해당 파일의 경로를 작성한다. 
  ```java
  // 스프링 초기 설정으로 생긴 Application 클래스와 같은 레벨에 클래스를 선언하였다.
  // ValueConfiguration.java
  
  @Configuration
  @PropertySource("classpath:application.properties")
  public class AppConfig { 
    // 아래 [3. 사용 예시] 에서 코드 추가 
  }
  ```

## 3. 사용 예시

* 가장 단순한 예시
  * 하드 코딩한 문자열 값을 주입한다.
  * 하드 코딩한 값을 사용하기 때문에 굳이 @PropertySource 를 선언하지 않는다.

  ```java
  @Configuration
  public class AppConfig {
  
    @Value("My String Value")
    private String stringValue;
    
    // 단순히 값의 주입 상태를 출력하기 위해 선언하였다.
    @Bean
    public void printValue() {
      System.out.println("stringValue = " + stringValue);
    }
  }
  ```
  * 스프링 애플리케이션을 실행(run)하면 stringValue 에 값이 채워져 아래와 같이 로그에 `stringValue = My String Value` 가 출력되는 것을 확인할 수 있다.
    <img width="474" alt="image" src="https://user-images.githubusercontent.com/49539592/143665902-9e1bed7b-3a76-40c0-9123-5af513404756.png">
    
* @PropertySource 을 사용한 예시 
  ```java
  @Configuration
  @PropertySource("classpath:application.properties")
  public class AppConfig {
  
    @Value("${value.from.file}")
    private String valueFromFile;  
  
    // 단순히 값의 주입 상태를 출력하기 위해 선언하였다.
    @Bean
    public void printValue() {
      System.out.println("valueFromFile = " + valueFromFile);
    }
  }
  ```
  * 스프링 애플리케이션을 실행(run)하면 [ 2. 애플리케이션 설정 ] 에서 정의한 바에 따라 `valueFromFile = Value got from the file`이 출력되는 것을 확인할 수 있다.
  <img width="473" alt="image" src="https://user-images.githubusercontent.com/49539592/143666285-4395aa84-8210-4b37-87a1-4ad8506c6bbc.png">
  
* 기본값 설정 예시
  * 정의되지 않은 속성에 대해 아래와 같이 기본값을 제공할 수도 있다.
  ```java
  @Configuration
  @PropertySource("classpath:application.properties")
  public class AppConfig {
  
    @Value("${unknown.param:some default}")
    private String someDefault;
  
    // 단순히 값의 주입 상태를 출력하기 위해 선언하였다.
    @Bean
    public void printValue() {
      System.out.println("someDefault = " + someDefault);
    }
  }
  ```
  * 스프링 애플리케이션을 실행(run)하면 정의되지 않은 경로의 값을 대신해 기본값이 사용되므로, 로그에 `someDefault = some default`가 출력되는 것을 확인할 수 있다.

* 시스템 속성 사용 예시
  * 예를 들어 `systemValue`라는 이름의 시스템 속성을 정의했다면 다음과 같이 사용할 수 있다.
    ```java
    @Value("${systemValue}")
    private String systemValue;
    ```
  * 만약 동일한 속성이 시스템 속성과 속성 파일에 정의되어 있으면 시스템 속성이 높은 우선순위로 적용된다. 
  * 시스템에서 priority 속성을 정의하고, 이 시스템 속성과 `application.properites`에서 이미 정의한 priority 속성 중 어떤 값이 출력되는지 확인해보자.
    ```java
    @Value("${priority}")
    private String prioritySystemProperty;
    ```
  * 위의 `prioritySystemProperty`을 출력하면 시스템 속성 값이 출력될 것이다(출력 이미지는 생략). 
* 리스트 / 집합 속성 사용 예시
  * 속성에 많은 값을 주입해야 하는 경우도 있다. 이 경우에는 속성 파일의 단일 속성에 쉼표로 구분된 값을 정의하거나 필요에 따라 시스템 속성을 정의할 수 있다. 
  * 아래에서는 위에서 정의한 listOfValues 의 값과 값 주입으로 생성된 클래스 이름을 출력해본다.
  * 또한 동일한 속성을 집합(Set) 타입으로도 선언하고 역시 값과 클래스 이름을 출력해본다.
  ```java
  @Configuration
  @PropertySource("classpath:application.properties")
  public class AppConfig {
  
    @Value("${listOfValues}")
    private List<String> listOfValues;
    
    @Value("${listOfValues}")
    private Set<String> setOfValues;
    
    // 단순히 값의 주입 상태를 출력하기 위해 선언하였다.
    @Bean
    public void printValue() {
      System.out.println("-----------------------------------------");
      System.out.println("listOfValues = " + listOfValues);
      System.out.println("list class = " + listOfValues.getClass());
      System.out.println("-----------------------------------------");
      System.out.println("setOfValues = " + setOfValues);
      System.out.println("set class = " + setOfValues.getClass());
      System.out.println("-----------------------------------------");
    }
  }
  ```
  * 스프링 애플리케이션을 실행(run)하면 아래와 같은 결과를 확인할 수 있다.
  <img width="469" alt="image" src="https://user-images.githubusercontent.com/49539592/143667010-afed3d67-3e9b-41fa-97d5-555964c34969.png">

* 맵(Map) 사용 예시
  * 먼저 속성 파일에서 `{ key: 'value' }` 형식으로 속성을 정의해야 한다.
  * 이때 Map의 값은 작은따옴표로 묶어야 한다.
  ```yaml
  // application.properites
  
  valuesMap={key1: '1', key2: '2', key3: '3'}
  ```
  * 이제 속성 파일에서 이 값을 Map으로 주입할 수 있다.
  ```java
  @Configuration
  @PropertySource("classpath:application.properties")
  public class AppConfig {
    
    // #={..} 는 스프링 표현언어(SpEL)이다. @Value 안에서 이렇게 쓸 수 있다는 것 정도만 알아두자.
    @Value("#{${valuesMap}}")
    private Map<String, Integer> valuesMap;
    
    // 단순히 값의 주입 상태를 출력하기 위해 선언하였다.
    @Bean
    public void printValue() {
    System.out.println("-----------------------------------------");
    System.out.println("valuesMap = " + valuesMap);
    System.out.println("map class = " + valuesMap.getClass());
    System.out.println("-----------------------------------------");
    }
  }
  ```
  * 스프링 애플리케이션을 실행(run)하면 아래와 같이 맵의 키 / 값, 그리고 클래스 이름이 출력된다.
  <img width="473" alt="image" src="https://user-images.githubusercontent.com/49539592/143668015-6bc1613c-0788-429b-9da5-7a3e32c407c1.png">
  * Map에서 특정 키의 값을 가져와야 하는 경우 표현식에 키 이름을 추가하면 된다.
    ```java
    @Value("#{${valuesMap}.key1}")
    private Integer valuesMapKey1;
    ```
    * valuesMapKey1 를 출력해보면 key1 의 값인 1이 출력된다.
  * 단, Map에서 키를 찾을 수 없을 경우를 대비해 값을 null로 설정하는 안전한 표현식을 선택해야 한다. 이렇게 하면 찾고자 하는 키가 Map에 포함되어 있지 않을 때 예외를 던지지 않는다.
    ```java
    @Value("#{${valuesMap}['unknownKey']}")
    private Integer unknownMapKey;
    ```
    * 위의 방식을 사용하면, `unknownKey`의 값이 있을 때 `unknownMapKey`에 해당 값을 주입하고 없을 때는 `null`을 주입한다.
  * 물론 존재하지 않을 수 있는 속성이나 키에 대한 기본값을 설정할 수도 있다.
    ```java
    @Value("#{${unknownMap : {key1: '1', key2: '2'}}}")
    private Map<String, Integer> unknownMap;

    @Value("#{${valuesMap}['unknownKey'] ?: 5}")
    private Integer unknownMapKeyWithDefaultValue;
    ```
  * unknownMap 과 unknownMapKeyWithDefaultValue 를 출력한 결과는 다음과 같다.
  <img width="470" alt="image" src="https://user-images.githubusercontent.com/49539592/143668319-d14e3370-7e78-4752-9c52-d0844a93ee4c.png">
    
  * Map 항목(Entry)은 주입 전에 필터링할 수도 있다.
  * 예를 들어 값이 1보다 큰 항목만 가져와야 한다고 가정해보자.
    ```java
    @Value("#{${valuesMap}.?[value>'1']}")
    private Map<String, Integer> valuesMapFiltered;
    ```
    * valuesMapFiltered 를 출력하면 다음과 같다.
    <img width="473" alt="image" src="https://user-images.githubusercontent.com/49539592/143668390-97f6d91b-0493-4f66-a95b-4093e36a367c.png">

* 생성자 주입 사용 예시
    * @Value 어노테이션은 필드 외에도 생성자 주입과 함께 사용할 수 있다. 다음 예시를 보자.
    ```java
    @Component
    @PropertySource("classpath:application.properties")
    public class PriorityProvider {
      
      private String priority;
    
      @Autowired
      public PriorityProvider(@Value("${priority:normal}") String priority) {
        this.priority = priority;
      }
    
      // standard getter
    }
    ```
    * 위의 예에서는 PriorityProvider의 생성자에 직접 `priority` 값을 주입한다. 속성을 찾을 수 없는 경우에는 기본값이 주입된다.
* 세터 주입 사용 예시
  * 생성자 주입과 유사하게 @Value를 setter 주입과 함께 사용할 수도 있다.
  ```java
  @Component
  @PropertySource("classpath:application.properties")
  public class CollectionProvider {
  
      private List<String> values = new ArrayList<>();
  
      @Autowired
      public void setValues(@Value("#{'${listOfValues}'.split(',')}") List<String> values) {
          this.values.addAll(values);
      }
  
      // standard getter
  }
  ```
  * 여기에서는 값 목록을 setValues 메소드에 삽입하기 위해 SpEL 표현식을 사용한다.

> [출처](https://www.baeldung.com/spring-value-annotation)




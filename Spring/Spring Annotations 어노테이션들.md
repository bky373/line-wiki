
## Spring Annotations

> 애노테이션, 어노테이션: 클래스, 인터페이스, 함수, 매개변수, 속성, 생성자에 어떤 의미를 추가할 수 있는 기능이며, 자바 컴파일러가 컴파일 시에 처리한다. 즉, 소스 코드에 추가된 애노테이션 자체는 바이트 코드로 생성되지 않고 주석으로 처리되지만, 그것이 갖는 의미대로 컴파일러가 작업을 수행해 준다. 애노테이션은 @로 시작하는 이름을 갖는다.

- **@RequestParam**   ( [출처](https://mangkyu.tistory.com/72) )
    - 1개의 **HTTP 파라미터**를 **변수**에 자동으로 바인딩한다.
    - **HTTP 파라미터**는 간단히 브라우저에서 서버로 전달되는 **키와 값의 쌍** 을 말한다. ( `key` : `value` )
      HTTP 요청 시,  파라미터는 `Path Parameter` 또는 `Query Parameter` 가 될 수 있다.
    - 사용시 필수 여부 ( `required` ) 의 기본값이 `true` 이기 때문에, 사용하려면 반드시 해당 파라미터가 전송되어야 한다. 해당 파라미터가 전송되지 않으면 `400 Error` 가 발생한다. 만약 반드시 필요한 변수가 아니라면 `required`의 값을 `false` 로 설정한다. 해당 변수가 없는 요청을 보낼 경우에 기본값을 설정하고 싶다면 `defaultValue` 값을 설정한다.
    - 스프링은 객체 타입이나 스칼라 타입일 경우 특별한 어노테이션 없이도 HTTP 파라미터를 자바 메소드의 파라미터로 변환해서 컨트롤러 메소드를 호출한다. 하지만  `Map` 타입의 경우는 예외여서 `@RequestParam` 어노테이션을 붙여야만 HTTP 파라미터의 값을 해당 `map` 안에 바인딩 해준다. ( 출처-1 )
- **@RequestBody** : 클라이언트가 전송하는 **JSON** ( `application/json` ) 형태의 **HTTP Body 내용**을 **Java Object** 로 변환해주는 역할을 한다. 만약 Body가 존재하지 않는 `GET` 메소드에 `@RequestBody` 를 활용하려고 하면 에러가 발생한다. `@RequestBody` 로 받는 데이터는 스프링에서 관리하는 `MessageConverter` 중 하나인 `MappingJackson2HttpMessageConverter` 를 통해 **Java Obejct** 로 변환된다.  ( [출처](https://mangkyu.tistory.com/72) )
- **@ModelAttribute** : 클라이언트가 전송하는 `multipart/form-data` 형태의 **HTTP Body 내용과 HTTP 파라미터들을 Setter 를 통해 1대1로 객체에 바인딩** 하기 위해 사용한다. `@ModelAttribute` 에는 매핑시키는 파라미터의 타입이 객체의 타입과 일치하는지를 포함한 다양한 검증 ( `Validation` ) 작업이 추가적으로 진행된다. 한 가지 예로, 게시물의 번호를 저장하는 `int` 형 `index` 변수에  "1번" 이라는 `String` 형을 넣으려고 하면, `BindException` 이 발생한다. `@ModelAttribute` 을 활용해서 특정 파라미터만을 받아올 수도 있다. 예를 들어 `{ country : "Korea" , capital : "Seoul" }` 형태의 데이터를 전송했다고 하면, 컨트롤러에서 `@ModelAttribute("capital") String captial` 의 형태로 `capital` 변수에  `Seoul`  만 바인딩시켜서 받아올 수 있다.  ( [출처](https://mangkyu.tistory.com/72) )
- **@RequestBody** 와  **@ModelAttribute**의 차이 : **@RequestBody** 는 요청받은 데이터를 변환하는 것이기 때문에 `Setter` 함수가 없어도 값이 매핑되지만, **@ModelAttribute** 는 바인딩하는 어떤 데이터를 set 하는 `Setter` 함수가 없다면 매핑이 되지 않는다.  ( [출처](https://mangkyu.tistory.com/72) )
- **@Transactional** : 일반적으로 스프링에서 `Service Layer` 에서  `@Transactional` 을 사용하여 트랜잭션 처리를 한다. 데이터 조회만 일어나는 `select` 메소드에서는 `@Transactional` 을 활용하지 않지만, 값을 추가하거나 변경 또는 삭제하는 `insert` ,  `update`,  `delete`  메소드에서는 `@Transactional` 을 추가하여 트랜잭션을 설정한다.  ( [출처](https://mangkyu.tistory.com/50?category=761302) )
- **@Valid** :  JSR-303 표준 스펙으로, **제약 조건이 부여된 객체**에 대해 빈 검증기 ( `Bean Validator` ) 가 검증하도록 지시하는 어노테이션이다. 스프링에서는 `LocalValidatorFactoryBean`을 이용해 JSR 표준의 검증 기능을 사용할 수 있는데, `LocalValidatorFactoryBean`은 JSR-303의 검증 기능을 이용할 수 있도록 도와주는 일종의 어댑터이다. JSR 표준의 빈 검증 기술의 특징은 객체의 필드에 달린 제약조건 어노테이션을 참고해 검증을 편리하게 할 수 있다는 것이다. 만약 검증에 오류가 있다면 `MethodArgumentNotValidException` 예외가 발생하고, 디스패처 서블릿에 기본으로 등록된 예외 리졸버 ( `Exception Resolver` ) 인 `DefaultHandlerExceptionResolver` 에 의해 `400 BadRequest` 에러가 발생한다. ( [출처](https://mangkyu.tistory.com/174?category=761302) )
    - JSR 표준 스펙은 다양한 제약 조건 어노테이션을 제공한다. 대표적인 어노테이션은 아래와 같다.
        - `@NonNull` : 해당 값이 `null`이 아닌지 검증함
        - `@AssertTrue` : 해당 값이 `true`인지 검증함
        - `@Size` : 해당 값이 주어진 값 사이에 해당하는지 검증함 ( `String`, `Collection`, `Map`, `Array` 에도 적용 가능함 )
        - `@Min` : 해당 값이 주어진 값보다 작지 않은지 검증함
        - `@Max` : 해당 값이 주어진 값보다 크지 않은지 검증함
        - 더 많은 어노테이션을 보고 싶다면, [자바 공식 문서](https://javaee.github.io/javaee-spec/javadocs/javax/validation/constraints/package-summary.html)(Java 8 기준)를 참고하자.
- **@Validated** : 객체를 검증하는 방법은 경우에 따라 달라질 수 있다. 예를 들어 일반 사용자의 요청과 관리자의 요청을 보내는 경우에 같은 객체로 요청이 오지만 다른 방식의 검증이 필요할 수 있다. 이런 경우에는 검증에 사용할 제약 조건이 2가지로 나뉘어야 한다. JSR-303 에서는 이런 경우를 위해 제약 조건 **어노테이션에 조건이 적용될 검증 그룹을 지정하여 적용** 할 수 있도록 **@Validated** 를 제공한다.  ( [출처](https://mangkyu.tistory.com/174?category=761302) )
- **@Repository** : 데이터에 접근하는 클래스임을 명시한다. 스프링이 데이터를 관리하는 클래스라고 인지해서 자바 빈 ( `Java bean` ) 으로 등록해서 관리한다.
- **@Autowired** : 멤버변수 위에 붙일 수 있으며 의존성을 주입하기 위해 사용한다. `@Autowired` 어노테이션을 붙긴 객체는 개발자가 `new` 키워드를 통해 직접 생성하지 않는다. 의존성 주입의 과정은 스프링에서 자동으로 실행되며, 개발자는 객체를 생성하는 일 없이 곧바로 사용할 수 있다. 그리고 멤버변수(필드) 위에  `@Autowired` 어노테이션으로 의존성을 주입하는 방식을 **필드 인젝션** ( `Field Injection` ) 이라 한다.
- **@RefreshScope**
    - 런타임에 빈이 동적으로 새로 고쳐지도록 하는 구현체이다. 빈이 고쳐진 후 빈에 접근하면 새로운 인스턴스가 생성된다.
    - Spring boot에서는 Config 를 쓰는 서버에서 내용을 변경할 때가 있다. @RefreshScope 어노테이션과 Actuator를 사용한다면 해당 변경사항을 클라이언트에 반영할 수 있다.
    - 이를 위해 Config 변경으로 영향을 받는 부분(클래스)에 @RefreshScope를 추가한다.  

* **@JsonProperty** :
    * 기존의 속성 또는 메서드를 논리적 속성 또는 메서드로 정의하여 사용할 때 쓰는 어노테이션이다. 
    * 직렬화/역직렬화 시 비정적(non-static) 객체 필드를 논리적 속성으로 정의할 수 있고, 
    * (시그니처에 따라) 논리적 속성에 대해 getter/setter 와 같은 비정적(non-static) 메서드를 정의할 수 있다.
    * 기본값("")일 경우 필드 이름 그대로 속성 이름이 되고, 값을 지정할 경우 필드 이름과 다른 이름을 지정할 수 있다. 속성 이름은 JSON 개체의 필드 이름으로 외부에서 사용되는 이름을 나타낸다.
    * com.fasterxml.jackson.annotation 에 속한다.
      ```java
        @Target(value={ANNOTATION_TYPE,FIELD,METHOD,PARAMETER})
        @Retention(value=RUNTIME)
        public @interface JsonProperty
      ```
    > [출처](https://fasterxml.github.io/jackson-annotations/javadoc/2.6/com/fasterxml/jackson/annotation/JsonProperty.html)

* **@JsonInclude**
    * 주석이 달려 있는 요소(필드/메서드/생성자 매개변수)나 주석이 달린 클래스의 모든 속성이 직렬화되는 시점에 사용된다.
    * 주석이 달려 있지 않은 경우 속성 값은 항상 포함된다. 하지만 이 주석을 사용하면 간단한 제외 규칙을 지정하여 직렬화할 속성의 양을 줄일 수 있다.
    * `JsonInclude.Include`는 직렬화 시 자바 빈의 어떤 속성을 포함할 것인지를 결정하는 열거 타입이다.
      * ALWAYS: 값과 상관없이 속성을 항상 포함한다.
      * NON_NULL: null 이 아닌 속성 만 포함한다.
      * NON_ABSENT: null 이 아닌 속성과 특정 참조 타입(자바 8의 Optional 또는 AtomicReference)의 absent value 가 아닌 속성 을 포함한다(NON_NULL에 포함되지 않는 요소들도 걸러준다.)
      * NON_EMPTY: null 또는 emtpty 로 여겨지는 속성을 제외한다. 이는 NON_NULL과 NON_ABSENT의 제외 범위에 더하여 데이터 타입 별로 empty 값이라 여겨질 만한 속성들을 제외한다. 예를 들어, Collection, Map의 isEmpty 에 해당하는 속성, 빈 배열, 빈 문자열 등이 그러하다.
      * 이외에도 NON_DEFAULT, CUSTOM, USE_DEFAULTS 상수가 있다.
    * com.fasterxml.jackson.annotation 에 속한다.
      ```java
      @Target(value={ANNOTATION_TYPE,METHOD,FIELD,TYPE,PARAMETER})
      @Retention(value=RUNTIME)
      public @interface JsonInclude
      ```
    > [출처](https://fasterxml.github.io/jackson-annotations/javadoc/2.9/com/fasterxml/jackson/annotation/JsonInclude.html)

* **@RestController**
    * @Controller 와 @ResponseBody 를 합쳐놓은 어노테이션이다.
    * 이 주석이 달린 타입은 하나의 컨트롤러 로 처리된다. 
    * 해당 컨트롤러에서 @RequestMapping 메서드들은 @ResponseBody의 의미체계(semantics)를 디폴트로 인지하고 동작한다.
    * org.springframework.web.bind.annotation 에 속한다.
      ```java
        @Target(value=TYPE)
        @Retention(value=RUNTIME)
        @Documented
        @Controller
        @ResponseBody
        public @interface RestController
      ```
    > [출처](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html)

* **@RestControllerAdvice**
    * @ControllerAdvice 와 @ResponseBody 를 합쳐놓은 어노테이션이다.
    * 이 주석이 달린 타입은 하나의 컨트롤러 어드바이스(controller advice) 로 처리된다.
    * 해당 컨트롤러에서 @ExceptionHandler 메서드들은 @ResponseBody의 의미체계(semantics)를 디폴트로 인지하고 동작한다.
    * 참고로, 이 주석은 HandlerMapping-HandlerAdapter 와 같은 페어(pair)가 적절히 구성된 경우에 처리된다(예를 들어 RequestMappingHandlerMapping-RequestMappingHandlerAdapter 과 같은 한 쌍).
    * org.springframework.web.bind.annotation 에 속한다.
      ```java
        @Target(value=TYPE)
        @Retention(value=RUNTIME)
        @Documented
        @ControllerAdvice
        @ResponseBody
        public @interface RestControllerAdvice
      ```
  > [출처](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestControllerAdvice.html)


* **@ControllerAdvice**
    * @Controller 이 달린 클래스들에 공유할 메서드들을 갖는 클래스에 붙이는 어노테이션이다. 공유할 메서드란  @ExceptionHandler, @InitBinder, @ModelAttribute 메서드들을 말한다. @Component에 전문성을 더했다고 보면 된다. 
      * org.springframework.web.bind.annotation 에 속한다.
      ```java
        @Target(value=TYPE)
        @Retention(value=RUNTIME)
        @Documented
        @Component
        public @interface ControllerAdvice
      ```
  > [출처](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html)

* **@Query**
    * repository 메서드에서 직접 파인더 쿼리(finder queries)를 선언하는 데 사용되는 어노테이션이다.
    * org.springframework.data.annotation 에 속한다.
    
* **@Retention**
    * 이 주석이 달린 타입의 어노테이션들이 얼마나 유지되는지를 나타낸다. 
    * 어노테이션 타입 선언에 Retention 주석이 없다면 기본적으로 보존 정책은 RetentionPolicy.CLASS로 설정된다.
    * Retention 메타 주석은 메타 주석 유형이 어노테이션에 직접 사용될 때만 효과가 있다. 메타 주석 유형이 다른 주석 유형의 멤버 유형으로 사용되는 경우에는 효과가 없다.
    * java.lang.annotation 에 속한다.
      ```java
        @Documented
        @Retention(value=RUNTIME)
        @Target(value=ANNOTATION_TYPE)
        public @interface Retention
      ```
    > [출처](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Retention.html)

    
* **RetentionPolicy**
    * 어노테이션 보존 정책으로 열거 타입이다. 이 열거형의 상수는 어노테이션을 유지하기 위한 정책을 설명한다. 
    * 어노테이션이 유지되는 기간을 지정하기 위해 Retention 메타 주석 유형과 함께 사용한다.
    * java.lang.annotation 에 속한다.
    * 상수의 종류 및 설명
      * CLASS: 어노테이션이 컴파일러에 의해 클래스 파일에 기록된다. 하지만 런타임에는 VM에 의해 유지될 필요가 없다.
      * RUNTIME: 어노테이션이 컴파일러에 의해 클래스 파일에 기록되고 런타임에 VM에 의해 유지되므로 반사적으로(reflectively) 읽을 수 있다.
      * SOURCE: 어노테이션이 컴파일러에 의해 삭제된다.
    > [출처](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html)

* **@Target**
    * 어노테이션 타입이 적용될 수 있는 컨텍스트를 나타낸다. 
    * 어노테이션 타입이 적용될 수 있는 선언 컨텍스트 및 유형 컨텍스트는 JLS 9.6.4.1에 지정되어 있으며 소스 코드에서 java.lang.annotation.ElementType의 열거형 상수로 표시된다(아래 예시 참고).
    * java.lang.annotation 에 속한다.
      ```java
        @Documented
        @Retention(value=RUNTIME)
        @Target(value=ANNOTATION_TYPE)
        public @interface Target
      ```
    * 예를 들어 @Target 메타 주석 자체는 선언된 타입 자체가 메타 주석 타입(meta-annotation type)임을 나타낸다. 따라서 이는 어노테이션 타입 선언(annotation type declaration)에만 사용할 수 있다.
      ```java
        @Target(ElementType.ANNOTATION_TYPE)
        public @interface MetaAnnotationType {
          ...
        }
      ```
    > [출처](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Target.html)

* **@DynamicUpdate**
    * 업데이트 시 값이 변경된 컬럼만을 사용하여 동적 SQL 문을 생성하는 어노테이션 이다. 엔티티 클래스 위에 붙여 사용한다.
    * org.hibernate.annotations 패키지에 속한다.
      ```java
        @Target(TYPE)
        @Retention(RUNTIME)
        public @interface DynamicUpdate
      ```
    > [출처](https://docs.jboss.org/hibernate/orm/6.0/javadocs/org/hibernate/annotations/DynamicUpdate.html)

* **@DynamicInsert**
    * 인서트(insert) 시 null이 아닌 컬럼만을 사용하여 동적 SQL 문을 생성하는 어노테이션 이다. 엔티티 클래스 위에 붙여 사용한다.
    * org.hibernate.annotations 패키지에 속해 있다.
      ```java
        @Target(value=TYPE)
        @Retention(value=RUNTIME)
        public @interface DynamicInsert
      ```
    > [출처](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/DynamicInsert.html)

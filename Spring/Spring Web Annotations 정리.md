# Spring Web Annotations

## 1. 개요

* `org.springframework.web.bind.annotation` 패키지에 속한 스프링 웹 어노테이션들에 대해 배웁니다.

## 2. @RequestMapping

* `@RequestMapping` 은 `@Controller` 클래스 내 request handler 역할을 하는 메소드를 표시하기 위해 사용합니다. 다음을 사용하여 구성할 수
  있습니다.
    * `path`, `name`, `value` : 메소드가 매핑되는 URL을 설정합니다.
    * `method`: 호환되는 HTTP 메소드를 설정합니다.
    * `params`: HTTP 매개변수의 존재여부 또는 매개변수의 값에 따라 요청을 필터링합니다.
    * `headers`: HTTP 헤더의 존재여부 또는 매개변수의 값에 따라 요청을 필터링합니다.
    * `consumes`: HTTP 요청 본문(request body)에서 메소드가 사용할 수 있는 미디어 타입을 설정합니다.
    * `produces`: HTTP 응답 본문(response body)에서 메소드가 생성할 수 있는 미디어 타입을 설정합니다.
* 다음은 `@RequestMapping` 을 사용하는 간단한 예시입니다.
    ```java
        @Controller
        class VehicleController {
        
            @RequestMapping(value = "/vehicles/home", method = RequestMethod.GET)
            String home() {
                return "home";
            }
        }
    ```
* 클래스 수준에서 이 어노테이션을 적용하면 **`@Controller` 클래스의 모든 핸들러 메소드에 대한 디폴트 설정을 제공할 수 있습니다.**
* 단, **URL 설정은 스프링이 재정의하지 않고, 경로를 나누어 구성합니다.**
* 예를 들어 다음 구성은 위와 동일한 효과를 가집니다.
    ```java
        @Controller
        @RequestMapping(value = "/vehicles", method = RequestMethod.GET)
        class VehicleController {
        
            @RequestMapping("/home")
            String home() {
                return "home";
            }
        }
    ```
* 스프링 4.3 릴리스 부터는 `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` 그리고 `@PatchMapping`
  을 사용할 수 있습니다.
* 이들은 `@RequestMapping`의 변형으로, `method` 값이 각각 GET, POST, PUT, DELETE 그리고 PATCH 로 설정되어 있습니다.

## 3. @RequestBody

* [@RequestBody](https://www.baeldung.com/spring-request-response-body) 어노테이션은 **HTTP 요청의 본문을 객체(
  object)에 매핑하는 데** 사용됩니다.
  ```java
    @PostMapping("/save")
    void saveVehicle(@RequestBody Vehicle vehicle) {
      // ...
    }
  ```
* 어노테이션을 사용하면, 요청(request)의 콘텐츠 유형(content type)에 맞게 역직렬화가 이루어집니다.

## 4. @PathVariable

* 이 어노테이션은 **메소드 인수가 URI 템플릿 변수에 바인딩되었음을 나타냅니다.**
* `@RequestMapping` 어노테이션으로 URI 템플릿을 지정하고 `@PathVariable` 을 사용하여 메소드 인수와 템플릿 변수 하나를 바인딩합니다.
* 이때 `name` 또는 `value` 인수를 사용하여 인수와 변수를 바인딩합니다(`name` 또는 `value`의 값은 템플릿 변수 이름을 사용합니다).
  ```java
    @RequestMapping("/{id}")
    Vehicle getVehicle(@PathVariable("id") long id) {
      // ...
    }
  ```
* 만약 템플릿에 있는 이름과 메소드 인수의 이름이 일치하면 어노테이션의 `name` 을 설정하지 않아도 됩니다.
  ```java
    @RequestMapping("/{id}")
    Vehicle getVehicle(@PathVariable long id) {
      // ...
    }
  ```
* 또한 `required` 값을 `false` 로 설정하여 경로 변수(path variable)를 선택적으로 표시할 수 있습니다.
  ```java
    @RequestMapping("/{id}")
    Vehicle getVehicle(@PathVariable(required = false) long id) {
      // ...
    }
  ```

## 5. @RequestParam

* `@RequestParam` 어노테이션은 **HTTP 요청 매개변수에 접근하기 위해** 사용합니다.
  ```java
    @RequestMapping
    Vehicle getVehicleByParam(@RequestParam("id") long id) {
      // ...
    }
  ```
* `@RequestParam` 어노테이션의 구성 옵션은 `@PathVariable` 어노테이션과 동일합니다.
* 또한, `@RequestParam`은 HTTP 요청에서 값이 없거나 비어있을 때 디폴트 값을 지정할 수 있습니다.
* 이때 사용하는 옵션은 `defaultValue` 이고, 디폴트 값을 설정하면 `required` 값이 암묵적으로 `false` 로 설정됩니다.
  ```java
    @RequestMapping("/buy")
    Car buyCar(@RequestParam(defaultValue = "5") int seatCount) {
      // ...
    }
  ```
* 매개변수 외에도, HTTP 요청 중 접근할 수 있는 다른 값들이 있습니다. 예를 들어 **쿠키나 헤더** 가 있는데, 이는 각각 `@CookieValue`
  및 `@RequestHeader` 어노테이션을 통해 접근할 수 있습니다.
* 이들 어노테이션의 구성 옵션도 `@RequestParam`의 것과 동일합니다.

## 6. 응답 처리 어노테이션들(Response Handling Annotations)

* 이번 섹션에서는 스프링 MVC에서 HTTP 응답을 조작하는 가장 일반적인 어노테이션들에 대해 알아봅니다.

### 6.1. @ResponseBody

* 요청 처리 메소드 위에 [@ResponseBody](https://www.baeldung.com/spring-request-response-body) 어노테이션을 붙이면 해당
  메소드의 결과를 응답 자체로 취급합니다.
  ```java
    @ResponseBody
    @RequestMapping("/hello")
    String hello() {
      return "Hello World!";
    }
  ```
* 만약 이 어노테이션을 `@Controller`가 달린 클래스에 추가하면, 해당 클래스의 모든 요청 처리 메소드는 자신의 반환값을 응답 자체로 처리할 수 있습니다.

### 6.2. @ExceptionHandler

* `@ExceptionHandler` 어노테이션을 붙인 메소드를 **커스텀 예외 처리 메소드** 로 선언할 수 있습니다.
* 커스텀 예외 처리 메소드로 선언이 되면, 요청 처리 메소드가 지정한 예외를 던질 때 이 메소드가 호출됩니다.
* catch된 예외는 메소드 인수로 전달할 수 있습니다. 다음 예시를 보겠습니다.
  ```java
    @ExceptionHandler(IllegalArgumentException.class)
    void onIllegalArgumentException(IllegalArgumentException exception) {
      // ...
    }
  ```

### 6.3. @ResponseStatus

* 요청 처리 메서드에 `@ResponseStatus` 어노테이션을 추가하면, **원하는 HTTP 응답 상태** 를 지정할 수 있습니다.
* 이때 사용할 인수 옵션은 `code` 또는 `value` 입니다.
* 추가적으로 `reason` 인수를 사용하여 이유를 제공할 수도 있습니다.
* 마지막으로 `@ExceptionHandler` 어노테이션과 함께 사용할 수도 있습니다.
  ```java
    @ExceptionHandler(IllegalArgumentException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    void onIllegalArgumentException(IllegalArgumentException exception) {
      // ...
    }
  ```
* 더 자세한 내용은 [이곳](https://www.baeldung.com/spring-mvc-controller-custom-http-status-code) 을 참조해주세요.

## 7. 다른 웹 어노테이션들(Other Web Annotations)

* 앞에서 살펴본 어노테이션들 외에 일반적으로 많이 사용되는 웹 어노테이션들을 소개합니다.

### 7.1. @Controller

* `@Controller` 어노테이션을 사용하여 스프링 MVC 컨트롤러를 정의할 수 있습니다.
* 자세한 내용은 [이곳](https://www.baeldung.com/spring-bean-annotations) 을 참조해주세요.

### 7.2. @RestController

* `@RestController` 는 **`@Controller` 와 `@ResponseBody`를 결헙한 어노테이션** 입니다.
* 따라서 다음은 동일한 내용을 선언한 것입니다.
  ```java
    @Controller
    @ResponseBody
    class VehicleRestController {
      // ...
    }
  ```
  ```java
    @RestController
    class VehicleRestController {
      // ...
    }
  ```

### 7.3. @ModelAttribute

* 이 어노테이션을 사용하면 MVC **`@Controller` 모델(객체) 요소에 접근할 수 있습니다.**
  ```java
    @PostMapping("/assemble")
    void assembleVehicle(@ModelAttribute("vehicle") Vehicle vehicleInModel) {
      // ...
    }
  ```
* 만약 인수의 이름이 같다면, `@PathVariable` 이나 `@RequestParam` 처럼 옵션을 생략할 수 있습니다.
  ```java
    @PostMapping("/assemble")
    void assembleVehicle(@ModelAttribute Vehicle vehicle) {
      // ...
    }
  ```
* 또한 메소드 위에 `@ModelAttribute` 을 추가하면 스프링은 자동으로 메소드의 반환 값을 모델에 추가합니다.
  ```java
    @ModelAttribute("vehicle")
    Vehicle getVehicle() {
      // ...
    }
  ```





## 참고 자료

* [Spring Web Annotations](https://www.baeldung.com/spring-mvc-annotations)

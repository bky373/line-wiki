# Spring Web Annotations

## 1. 개요
* `org.springframework.web.bind.annotation` 패키지에 속한 스프링 웹 어노테이션들에 대해 배웁니다.

## 2. @RequestMapping
* `@RequestMapping` 은 `@Controller` 클래스 내 request handler 역할을 하는 메서드를 표시하기 위해 사용합니다. 다음을 사용하여 구성할 수 있습니다.
    * `path`, `name`, `value` : 메서드가 매핑되는 URL을 설정합니다.
    * `method`: 호환되는 HTTP 메서드를 설정합니다.
    * `params`: HTTP 매개변수의 존재여부 또는 매개변수의 값에 따라 요청을 필터링합니다.
    * `headers`: HTTP 헤더의 존재여부 또는 매개변수의 값에 따라 요청을 필터링합니다.
    * `consumes`: HTTP 요청 본문(request body)에서 메서드가 사용할 수 있는 미디어 타입을 설정합니다.
    * `produces`: HTTP 응답 본문(response body)에서 메서드가 생성할 수 있는 미디어 타입을 설정합니다.
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
* 클래스 수준에서 이 어노테이션을 적용하면 **`@Controller` 클래스의 모든 핸들러 메서드에 대한 디폴트 설정을 제공할 수 있습니다.** 
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
* 스프링 4.3 릴리스 부터는 `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` 그리고 `@PatchMapping` 을 사용할 수 있습니다.
* 이들은 `@RequestMapping`의 변형으로, `method` 값이 각각 GET, POST, PUT, DELETE 그리고 PATCH 로 설정되어 있습니다. 

## 3. @RequestBody
* [@RequestBody](https://www.baeldung.com/spring-request-response-body) 어노테이션은 **HTTP 요청의 본문을 객체(object)에 매핑하는 데** 사용됩니다.
  ```java
    @PostMapping("/save")
    void saveVehicle(@RequestBody Vehicle vehicle) {
      // ...
    }
  ```
* 어노테이션을 사용하면, 요청(request)의 콘텐츠 유형(content type)에 맞게 역직렬화가 이루어집니다.

## 4. @PathVariable
* 이 어노테이션은 **메서드 인수가 URI 템플릿 변수에 바인딩되었음을 나타냅니다.**
* `@RequestMapping` 어노테이션으로 URI 템플릿을 지정하고 `@PathVariable` 을 사용하여 메서드 인수와 템플릿 변수 하나를 바인딩합니다.
* 이때 `name` 또는 `value` 인수를 사용하여 인수와 변수를 바인딩합니다(`name` 또는 `value`의 값은 템플릿 변수 이름을 사용합니다).
  ```java
    @RequestMapping("/{id}")
    Vehicle getVehicle(@PathVariable("id") long id) {
      // ...
    }
  ```
* 만약 템플릿에 있는 이름과 메서드 인수의 이름이 일치하면 어노테이션의 `name` 을 설정하지 않아도 됩니다.
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



## 참고 자료
* [Spring Web Annotations](https://www.baeldung.com/spring-mvc-annotations)

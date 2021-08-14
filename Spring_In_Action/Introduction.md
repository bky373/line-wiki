# Spring in Action

## 책 정보

- 저자 : 크레이그 월즈
- 옮긴이 : 심채철
- 출판사 : 제이펍
- 출간일 : 2020년 05월 14일
- 에디션 : 5th Edition
- 관련링크 :  [실습용 소스 코드](https://github.com/Jpub/SpringInAction5)

## 1.1 스프링이란?(p4~6)

스프링은 **스프링 애플리케이션 컨텍스트**(Spring application context) (**컨테이너**) 를 제공하고, 이는 애플리케이션 컴포넌트들을 생성하고 관리한다. 그리고 **애플리케이션 컴포넌트** 또는 **빈**(bean)들은 스프링 애플리케이션 컨텍스트 내부에서 서로 연결되어 완전한 애플리케이션을 만든다.

빈의 상호 연결은 **의존성 주입**(Dependency Injection, DI)이라고 알려진 패턴을 기반으로 수행된다. 즉, 애플리케이션 컴포넌트에서 의존(사용)하는 다른 빈의 생성과 관리를 개발자가 작업하는 대신 스프링이 컨테이너가 해주며, 이 개체(컨테이너)에서는 모든 컴포넌트를 생성, 관리하고 해당 컴포넌트를 필요로 하는 빈에 주입(연결)한다. 일반적으로 이 작업은 생성자 인자 또는 속성의 접근자 메서드를 통해 처리된다. 

과거의 스프링 버전에서는 컴포넌트 및 다른 컴포넌트와의 관계를 나타내기 위해 하나 이상의 XML 파일을 사용하였다. 

하지만 최신 버전의 스프링에서는 자바 기반의 구성(configuration)을 더 많이 사용한다. 자바 기반 구성 클래스의 예시는 다음과 같다.

```java
@Configuration
public class ServiceConfiguration {
    @Bean
    public InventoryService inventoryService() {
        return new InventoryService();
    }
    @Bean
    public ProductService productService() {
        return new ProductService(inventoryService());
    }
}
```

여기서 @Configuration 애노테이션은 각 빈이 스프링 애플리케이션 컨텍스트에 제공하는 구성 클래스라는 것을 스프링에게 알려준다. 위의 클래스의 메서드에는 @Bean 애노테이션이 지정되어 있는데, 이는 각 메서드의 **반환 객체가 애플리케이션 컨텍스트의 빈** 으로 추가되어야 함을 나타낸다. 

참고로 XML 기반 구성이나 위의 자바 기반 구성은 스프링의 자동-구성을 할 수 없을 경우에 사용한다. 자동-구성은 스프링의 자동 연결(autowiring) 및 컴포넌트 검색(component scanning) 기술을 기반으로 한다. 컴포넌트 검색을 사용하여 스프링은 자동으로 애플리케이션의 **classpath에 지정된 컴포넌트를 찾은 후 스프링 애플리케이션 컨텍스트의 빈으로 생성** 할 수 있다. 또한, **자동 연결을 사용하여 의존 관계가 있는 컴포넌트를 자동으로 다른 빈에 주입(연결)** 한다.

최근에는 **스프링 부트(Spring Boot)** 가 소개되면서 자동-구성 기능이 더욱 향상되었다. 스프링 부트는 **생산성 향상을 제공하는 스프링 프레임워크의 확장** 이며, 향상된 자동-구성 기능 덕분에 환경 변수 classpth를 기준으로 어떤 컴포넌트가 구성되고 연결되어야 하는지 알 수 있다.

## 1.2 스프링 애플리케이션 초기 설정하기(p7)

스프링 Initializr(이니셜라이저)를 사용하면 애플리케이션의 초기 설정을 쉽게 할 수 있다. 이를 통해 애플리케이션의 **프로젝트 디렉터리 구조 생성** 과  **빌드 명세 정의**에 드는 시간을 확 줄일 수 있다.

- 스프링 Initializr는 REST API를 사용하는 브라우저 기반의 웹 애플리케이션이며, 스프링 프로젝트의 구조를 생성해준다. 

## 1.2.1 STS를 사용해서 스프링 프로젝트 초기 설정하기(p8~p12)

STS는 이클립스를 기반으로 하는 스프링 IDE이다. 스프링 부트 대시보드 기능을 제공한다.

JAR(Java ARchive) 패키징은 클라우드를 염두에 둔 선택이다. WAR(Web application ARchive) 파일은 기존의 자바 애플리케이션 서버에 애플리케이션을 배포할 때는 적합하지만, 대부분의 클라우드 플랫폼에는 잘 맞지 않는다. 모든 자바 클라우드 플랫폼은 실행 가능한 JAR 파일을 사용한다. 스프링 Initializr에서도 JAR 패키징을 기본값으로 사용한다.

스프링 Initializr에서 프로젝트에 추가할 의존성(dependency)을 지정할 수 있다(의존성에는 프로젝트의 애플리케이션에서 사용할 외부 라이브러리 모듈을 지정하며, 여러 컴포넌트 클래스가 포함된 jar 파일이 주로 사용된다).

## 1.2.2 스프링 프로젝트 구조 살펴보기 (p12~18)

전형적인 **메이븐 또는 그래들 프로젝트의 기본 구조** 는 다음과 같다.

- `src/main/java`: 애플리케이션 소스 코드
- `src/main/resources`: 자바 리소스가 아닌 것
- `src/test/java`: 애플리케이션 테스트 코드

아래는 **메이븐 프로젝트 구조** 에서 추가적으로 살펴볼 수 있는 항목들이다.

-  `mvnw`와 `mvnw.cmd`: 메이븐 래퍼 스크립트이다. 각자 컴퓨터에 메이븐이 설치되어 있지 않더라도 이 스크립트를 사용하여 프로젝트를 빌드할 수 있다.
- `pom.xml`: pom은 project object model의 약자로, **메이븐 빌드 명세** (프로젝트를 빌드할 때 필요한 정보)를 지정한 파일이다.
- `TacoCloudApplication.java`: 스프링 부트 메인 클래스이다. `Application` 앞의 `TacoCloud`는 본서에서 다루는 프로젝트의 이름이다. 각자 진행하는 프로젝트 이름 뒤에 `Application`이 붙게 되는 자바 파일이다. 
- `application.properties`: 처음에는 파일 내용이 없지만, 구성 속성을 커스텀하게 지정할 수 있다.
- `static`: 브라우저에 제공할 정적인 콘텐츠(이미지, 스타일시트, 자바스크립트 등)를 두는 폴더이다. 처음에는 비어 있다.
- `templates`: 브라우저에 콘텐츠를 보여주는 템플릿 파일을 두는 폴더이다. 처음에는 비어 있다.
- `TacoCloudApplicationTests.java`: 스프링 애플리케이션이 성공적으로 로드되는지 확인하는 간단한 테스트 클래스이다.

**스프링 부트 스타터(spring-boot-starter)**  의존성을 사용할 때 다음과 같은 장점을 가진다.

- 필요로 하는 모든 라이브러리의 의존성을 선언하지 않아도 되므로 빌드 파일이 훨씬 더 작아지고 관리하기 쉬워진다.
- 라이브러리 이름이 아닌 기능의 관점으로 의존성을 생각할 수 있다. 만약 웹 애플리케이션을 개발한다면 웹 애플리케이션 관련 라이브러리를 하나하나 지정하는 대신 웹 스타터 의존성만 추가하면 된다.
- 라이브러리들의 버전을 걱정하지 않아도 된다. 스프링 부트에 포함된 라이브러리들의 버전은 호환이 보장되기 때문에 사용하려는 스프링 부트 버전만 신경 쓰면 된다.

## **애플리케이션의 부트스트랩(구동)(p16-17)**

실행 가능 JAR 파일에서 애플리케이션을 실행하므로 제일 먼저 시작되는 **부트스트랩 클래스** 가 있어야 한다. 또한 애플리케이션을 부트스트랩하기 위한 최소한의 스프링 구성도 있어야 한다. 본서의 부트스트랩 클래스인 TacoCloudApplication의 내용은 다음과 같다.

```java
package tacos;

import org.springframework.boot.SpringBootApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;


@SpringBootApplication // 스프링 부트 애플리케이션
public class TacoCloudApplication(String[] args) {
  
  public static void main(String[] args) {
    SpringApplication.run(TacoCloudApplicatoin.class, args); // 애플리케이션을 실행한다.
  }
}
```

@SpringBootApplicaton은 다음 세 개의 애노테이션을 결합한 것이다.

- @SpringBootConfiguration: 현재 클래스(TacoCloudConfiguration)를 구성 클래스로 지정한다. 필요하다면 자바 기반의 스프링 프레임워크 구성을 현재 클래스에 추가할 수 있다. 실제로는 이 애노테이션은 @Configuration 의 특화된 형태라고 할 수 있다.
- @EnableAutoConfiguration: 스프링 부트 자동-구성을 활성화한다. 이 애노테이션은 우리가 필요로 하는 컴포넌트들을 자동으로 구성하도록 스프링 부트에 알려준다.
- @ComponentScan: 컴포넌트 검색을 활성화한다. 이것은 @Component, @Controller, @Service 등의 애노테이션과 함께 클래스를 선언할 수 있게 해준다. 그러면 스프링은 자동으로 그런 클래스를 찾아 스프링 애플리케이션 컨텍스트에 컴포넌트로 등록한다.

TacoCloudApplication의 또 다른 중요한 부분은 **main() 메서드** 이다. 이것은 **JAR 파일이 실행될 때 호출되어 실행되는 메서드** 이다. main() 메서드는 실제로 애플리케이션을 시작시키고 스프링 애플리케이션 컨텍스트를 생성하는 run() 메서드를 호출한다. 

run() 메서드에 전달되는 두 개의 매개변수는 구성 클래스와 명령행(command-line) 인자이다. 구성 클래스가 부트스트랩 클래스와 반드시 같아야 하는 것은 아니지만 대개 동일하게 지정한다.

## 1.3 스프링 애플리케이션 작성하기(p18~)

먼저 작성해볼 두 가지 코드

- 홈페이지의 웹 요청(request)을 처리하는 컨트롤러(controller) 클래스
- 홈페이지의 모습을 정의하는 뷰 템플릿

## 1.3.1 웹 요청 처리하기(p19~20)

스프링은 스프링 MVC라는 강력한 웹 프레임워크를 가지고 있다. 스프링 MVC의 중심에는 **웹 요청과 응답을 처리하는 컴포넌트, 컨트롤러** 가 있다. 웹 브라우저를 상대하는 애플리케이션의 컨트롤러는 선택적으로 모델 데이터를 채워서 응답할 수 있다. 여기에서는 루트 경로( `/` )의 웹 요청을 처리한 후 모델 데이터를 채우지 않고 해당 웹 요청을 뷰로 전달하는 컨트롤러 클래스를 아래와 같이 작성할 것이다.

```java
package tacos;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller // 컨트롤러
public class HomeController {
    
    @GetMapping("/") // 루트 경로인 /의 웹 요청을 처리한다.
    publc String home() {
        return "home"; // 뷰 이름을 반환한다.
    }
}
```

@Controller는 해당 클래스가 컴포넌트로 식별되게 해준다. 스프링의 컴포넌트 검색시 HomeController는 스프링 애플리케이션 **컨텍스트의 빈으로서 인스턴스가 생성** 된다. 

@Controller 외에 @Component, @Service, @Repository 등 소수의 애노테이션도 동일한 기능을 제공한다. 하지만 **@Controller를 사용하는 것으로 애플리케이션 내 해당 컴포넌트의 역할을 더 잘 설명** 할 수 있다.

루트 경로인 `/` 로 HTTP GET 요청이 들어오면 home() 메서드가 요청을 처리해야 한다. 여기에서는 home 값을 갖는 String만 반환하고 다른 일은 하지 않는다. 이 값은 뷰의 논리적인 이름이다. 뷰는 JSP나 FreeMarker 등 여러 방법으로 구현할 수 있지만, 본서에서는 Thymeleaf를 사용하여 뷰 템플릿을 정의한다.

논리적인 뷰 이름(여기에서는 home) 앞에 /templates/가 붙고 끝에는 .html이 추가된 것이 템플릿 경로와 파일 이름이 되기 때문에 여기에서는 /templates/home.html이 된다. 본서의 프로젝트에서는 /src/main/resources/templates/home.html이 home 메서드의 뷰 템플릿 역할을 한다.

## 1.3.2 뷰 정의하기(p20~21)

아래는 타코 클라우드 홈페이지 템플릿이다.

```java
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="EUC-KR">
	<title>Taco Cloud</title>
  </head>
          
  <body>
    <h1>Welcome to...</h1>
    <img th:src="@{/images/TacoCloud.png}"/>
  </body>
</html>
```

 `<img>` 태그 안에 @{...} 표현식을 사용해 컨텍스트의 상대적인 경로에 위치하는 이미지를 참조하는 th:src 속성 값을 지정할 수 있다. 이미지와 같은 정적 콘텐츠는 /src/main/resources/static 폴더에 위치해야 한다. 

## 1.3.3 컨트롤러 테스트하기(p22~24)

아래에서는 지금까지 작업한 홈페이지를 테스트한다. 아래 코드를 통해 루트 경로인 `/`의 HTTP GET 요청을 성공적으로 수행하는지, 뷰 이름이 home이고 'Welcome to...' 메시지가 포함된 결과가 제대로 나오는지를 확인할 수 있다. 

```java
package tacos;

import static org.hmacrest.Matchers.containsString;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestMatchers.content;
import static org.springframework.test.web.servlet.request.MockMvcRequestMatchers.status;
import static org.springframework.test.web.servlet.request.MockMvcRequestMatchers.view;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

@WebMvcTest(HomeController.class) // HomeController의 웹 페이지 테스트
public class HomeControllerTest {
    
    @Autowired
    private MockMvc mockMvc; // MockMvc를 주입한다.
    
    @Test
    public void testHomePage() throws Exception {
        mockMvc.perform(get("/"))  // GET /를 수행한다.
            .andExpect(status().isOk()) // HTTP 200이 되어야 한다.
            .andExpect(view().name("home")) // home 뷰가 있어야 한다.
            .andExpect(content().string( // 콘텐츠에 'Welcome to...'가 포함되어야 한다.
            	containsString("Welcome to..."))); 
            
    }
}
```

위의 테스트에서는 @SpringBootTest 대신 @WebMvcTest 를 사용한다. 이는 스프링 MVC 애플리케이션의 형태로 테스트가 실행되도록 한다. 즉, HomeController가 스프링 MVC의 컨트롤러로 등록되어 우리는 스프링 MVC에 웹 요청을 보낼 수 있다.

@WebMvcTest 는 또한 스프링 MVC를 테스트 하기 위한 스프링 지원을 설정한다. 테스트에서는 굳이 무거운 실제 서버를 띄우지 않아도 된다. 대신 스프링 MVC의 모의(mocking) 메커니즘을 사용하는 것으로도 충분하다. 위의 테스트 클래스에서는 모의 테스트를 진행하기 위해 MockMvc 객체를 주입(연결)하였다.

## 1.3.4 애플리케이션 빌드하고 실행하기(p24~26)

>  본서에서는 이클립스 기반의 STS IDE를 위주로 설명하고 있는데 자신이 사용하는 IDE에서 사용하는 시작(start or run) 버튼을 눌러 애플리케이션을 실행하면 된다.

애플리케이션이 시작되면 작업 진행을 나타내는 로그들이 콘솔에 나타난다. 그중에는 톰캣(tomcat) 애플리케이션 서버를 자동으로 시작하는 Tomcat started on port(s): 8080 (http) 라는 로그가 있다. 해당 작업이 진행되면 홈페이지를 웹 브라우저에서 사용할 수 있다.

**톰캣에 애플리케이션을 따로 설치하지 않았는데도 실행이 되는 이유는 무엇일까?**

스프링 부트 애플리케이션에는 애플리케이션 실행에 필요한 모든 것이 포함된다. 따라서 톰캣과 같은 애플리케이션 서버에 애플리케이션을 따로 설치할 필요가 없다. 톰캣이 우리 애플리케이션의 일부라고 생각하면 된다.

이제 http://localhost:8080에 접속하면 웹 브라우저에서 직접 만든 애플리케이션을 사용할 수 있다.  

## 1.3.5 스프링 부트 DevTools 알아보기(p26~27)

DevTools는 개발 시점에서 다음의 편리한 기능을 제공한다. (실제 운영에서는 스스로 비활성화된다.)

- 코드가 변경될 때 자동으로 애플리케이션을 다시 시작한다.
- 브라우저로 전송되는 리소스(예를 들어 템프릿, 자바스크립트, 스타일시트)가 변경될 대 자동으로 브라우저를 새로고침한다.
- 템플릿 캐시를 자동으로 비활성화한다.
- 만약 H2 데이터베이스를 사용한다면 자동으로 H2 콘솔을 활성화한다.

## 자동으로 애플리케이션 다시 시작시키기(p27)

DevTools는 변경을 감시하기 때문에 무언가 변경되었을 때 자동으로 애플리케이션을 다시 시작한다. 더 자세히 말하면 DevTools를 사용할 때 애플리케이션은 JVM에서 두 개의 클래스 로더에 의해 로드된다. 하나는 자바 코드, 속성 파일, 프로젝트의 src/main/ 경로에 있는 모든 내용들이 함께 로드된다. 참고로 이들은 자주 변경될 수 있는 것들이다. 나머지 클래스 로더는 자주 변경되지 않는 의존성 라이브러리와 함께 로드된다. 

변경이 감지되면 DevTools는 프로젝트 코드를 포함하는 클래스 로더만 다시 로드하고 스프링 애플리케이션 컨텍스트를 다시 시작한다. 하지만 다른 클래스 로더와 JVM은 그대로 둔다. 이로써 애플리케이션 시작 시간을 조금이라도 단축하게 해준다.

다만 애플리케이션이 자동으로 다시 시작될 때 의존성 변경을 적용할 수 없다는 단점이 있다. 의존성 라이브러리를 포함하는 클래스 로더는 자동으로 다시 로드되지 않기 때문이다. 따라서 빌드 명세에 의존성을 추가, 변경, 삭제할 경우 애플리케이션을 수동으로 새로 시작해야 변경사항이 반영된다.

## 자동으로 브라우저를 새로고침하고 템플릿 캐시를 비활성화하기(p27~28)

기본적으로 Thymeleaf나 FreeMarket과 같은 템플릿은 템플릿의 파싱(코드 분석) 결과를 캐시에 저장하고 사용한다. 템플릿이 사용되는 모든 웹 요청마다 매번 파싱을 다시 하지 않도록 말이다. 

하지만 개발 시점에서는 템플릿 캐싱이 유용하지 않다. 템플릿을 변경하고 브라우저를 새로고침해도 여전히 변경 이전의 캐싱된 템플릿이 사용되기 때문이다. 이 경우 애플리케이션을 다시 시작해야만 변경된 결과를 볼 수 있다.

DevTools는 모든 템플릿 캐싱을 자동으로 비활성화하여 위의 문제를 해결한다. 따라서 템플릿을 얼마든지 변경하여도 브라우저만 새로고침해주면 변경된 템플릿이 적용된다.

더 나아가 브라우저 새로고침 버튼을 누리지 않고도 변경된 결과를 보는 방법이 있다. DevTools가 사용될 때는 애플리케이션과 함께 자동으로 LiveReload 서버를 활성화한다.  이 서버와 부합되는 LiveReload 브라우저 플러그인과 연결될 때는 브라우저에 전달되는 거의 모든 것(템플릿, 이미지, 스타일시트, 자바스크립트 등)이 변경될 때 브라우저가 자동으로 새로고침된다.

## 도메인 객체에 애노테이션 추가하기(p104~)

특정 클래스를 JPA 개체(entity)로 선언하려면 반드시 @Entity 애노테이션을 추가해야 한다. 그리고 이것의 id 속성에는 반드시 @Id 를 지정하여 이 속성이 데이터베이스의 개체를 고유하게 식별한다는 것을 나타내야 한다. 

JPA에서는 개체가 인자 없는 생성자를 가져야 한다. 이를 위해 Lombok의 @NoArgsConstructor를 지정한다. 하지만 인자 없는 생성자의 사용을 원하지 않을 경우, access 속성을 AccessLevel.PRIVATE으로 설정하여 클래스 외부에서 사용하지 못하게 만들 수 있다. 그리고 초기화가 필요한 final 속성이 있다면 force 속성을 true로 설정한다. 이에 따라 Lombok이 자동 생성한 생성자에서 그 속성들을 null로 설정할 수 있다. 아래 예시처럼 작성하면 된다.

```java
@NoArgsConstructor(access=AccessLevel.PRIVATE, force=true)
```

## JPA 리퍼지터리 선언하기(p108~)

JDBC 버전의 리퍼지터리에서는 리퍼지터리가 제공하는 메서드를 개발자가 명시적으로 선언한다.

하지만 스프링 데이터에서는 그 대신 CrudRepository 인터페이스를 확장(extends)할 수 있다. 예를 들어 다음 코드에서는 새로운 인터페이스인 IngredientRepository를 보여준다. CrudRepository 인터페이스에는 데이터베이스의 CRUD(Create(생성), Read(읽기), Update(변경), Delete(삭제)) 연산을 위한 많은 메서드가 선언되어 있다. 

```java
package tacos.data;

import org.springframework.data.repository.CrudRepository;

import tacos.Ingredient;

public interface IngredientRepository extends CrudRepository<Ingredient, String> {
    
    // Iterable<Ingredient> findAll();
    // Ingredient findById(String id);
    // Ingredient save(Ingredient ingredient);
}
```

CrudRepository 인터페이스의 첫 번째 매개변수는 리퍼지터리에 저장되는 개체 타입이고, 두 번째 매개변수는 개체 id 속성의 타입이다. 

인터페이스와 이에 정의된 메소드의 구현을 위해 클래스 구현체를 작성해야 한다고 생각할 수 있다. 하지만 그럴 필요가 없다. 애플리케이션이 시작될 때 스프링 데이터 JPA는 각 인터페이스 구현체(클래스 등)를 자동으로 생성해주기 때문이다. 즉 스프링 데이터 JPA를 사용하면 리퍼지터리만 작성하면 된다. 그리고 JDBC 기반의 구현에서 하듯이 리퍼지터리를 필요한 곳에 주입해주면 된다. 

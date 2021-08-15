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
-  `pom.xml`: pom은 project object model의 약자로, **메이븐 빌드 명세** (프로젝트를 빌드할 때 필요한 정보)를 지정한 파일이다.
-  `TacoCloudApplication.java`: 스프링 부트 메인 클래스이다. `Application` 앞의 `TacoCloud`는 본서에서 다루는 프로젝트의 이름이다. 각자 진행하는 프로젝트 이름 뒤에 `Application`이 붙게 되는 자바 파일이다. 
-  `application.properties`: 처음에는 파일 내용이 없지만, 구성 속성을 커스텀하게 지정할 수 있다.
-  `static`: 브라우저에 제공할 정적인 콘텐츠(이미지, 스타일시트, 자바스크립트 등)를 두는 폴더이다. 처음에는 비어 있다.
-  `templates`: 브라우저에 콘텐츠를 보여주는 템플릿 파일을 두는 폴더이다. 처음에는 비어 있다.
-  `TacoCloudApplicationTests.java`: 스프링 애플리케이션이 성공적으로 로드되는지 확인하는 간단한 테스트 클래스이다.

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

## H2 콘솔(p28)

만약 H2 데이터베이스를 사용한다면, 웹 브라우저에서 사용할 수 있는 H2 콘솔도 DevTools가 자동으로 활성화 해준다. 웹 브라우저에서 http://localhost:8080/h2-console에 접속하면 애플리케이션에서 사용하는 데이터를 확인할 수 있다.

## 1.3.6 리뷰하기(p28~30)

지금까지 타코 클라우드 애플리케이션을 구축하기 위해 다음과 같은 작업 단계를 수행하였다.

- 스프링 Initializr를 사용하여 프로젝트 초기 구조를 생성하였다.
- 홈페이지 웹 요청을 처리하기 위해 컨트롤러 클래스를 작성하였다.
- 홈페이지를 보여주기 위해 뷰 템플릿을 정의하였다.
- 애플리케이션을 테스트하기 위해 간단한 테스트 클래스를 작성하였다.

스프링을 사용한 애플리케이션 개발의 장점은 다음과 같다. 프레임워크의 요구를 만족시키기 위한 코드보다 **애플리케이션의 요구를 충족하는 코드에 집중할 수 있다**. 그런데 어떻게 이런 일이 가능할까? 스프링이 하는 일을 이해하기 위해 빌드 명세를 살펴보자.

빌드 명세(예 pom.xml)를 보면 Web과 Thymeleaf 의존성이 선언되어 있다. 이 두 가지 의존성은 다음 내용을 비롯하여 일부 다른 의존성을도 포함한다.

-  스프링의 MVC 프레임워크
-  내장된 톰캣
-  Thymeleaf와 Thymeleaf 레이아웃 dialect

스프링 부트의 자동-구성 라이브러리도 개입되기 때문에 **애플리케이션이 시작될 때 스프링부트 자동-구성에서 그런 의존성 라이브러리들을 감지하고 자동으로 다음의 일을 수행한다**.

- **스프링 MVC** 를 활성화하기 위해 스프링 애플리케이션 컨텍스트에 관련된 빈들을 구성한다.
- 내장된 **톰캣 서버** 를 스프링 애플리케이션 컨텍스트에 구성한다.
- Thymeleaf 템플릿을 사용하는 **스프링 MVC 뷰** 를 나타내기 위해 Thymeleaf 뷰 리졸버(resolver)를 구성한다.

요약하면 자동-구성이 모든 작업을 수행하기 때문에 우리는 애플리케이션 구현 코드를 작성하는 데 집중할 수 있는 것이다. 

## 1.4 스프링 살펴보기(p30~p33)

스프링에서 사용할 수 있는 의존성은 매우 다양하다. 여기에서는 몇 가지 중요한 것만 언급한다.

## 1.4.1 핵심 스프링 프레임워크(p30)

핵심 스프링 프레임워크는 **스프링에 있는 모든 것의 기반** 이며, **핵심 컨테이너** 와  **의존성 주입 프레임워크** 외에 몇 가지 다른 기능을 제공한다.

그중 하나가 스프링의 웹 프레임워크인 **스프링 MVC** 이다. 웹 요청을 처리할 때 HTML 응답이 아닌 REST API를 만들 때도 스프링 MVC를 사용할 수 있다.

핵심 스프링 프레임워크는 템플릿 기반의 JDBC 지원(JdbcTemplate)을 포함하여 기본적인 **데이터 퍼시스턴스 지원** 도 제공한다.

스프링의 가장 최신 버전에서는 리액티브 프로그래밍 지원이 추가되었다. 여기에서는 스프링 MVC 개념의 스프링 WebFlux라는 새로운 리액티브 웹 프레임워크가 포함된다.

## 1.4.2 스프링 부트(p31)

스프링 부트는 **스타터 의존성**과 **자동-구성** 을 포함하여 개발자가 애플리케이션 코드에 보다 집중할 수 있도록 도와준다. 이외에도 스프링 부트는 다음의 편리한 기능들을 함께 제공한다.

- 액추에이터(Actuator)는 애플리케이션의 내부 작동을 런타임에 살펴볼 수 있게 해준다. 여기에는 메트릭(metric), 스레드 덤프 정보, 애플리케이션 상태, 애플리케이션에서 사용할 수 있는 환경 속성이 포함된다.
- 환경 속성의 명세
- 핵심 프레임워크에 추가되는 테스트 지원

또한 스프링 부트는 스프링 부트 CLI(명령행 인터페이스)를 제공한다. 스프링 부트 CLI를 사용하면 애플리케이션 전체를 그루비 스크립트로 작성하여 명령행에서 실행할 수 있다. 

## 1.4.3 스프링 데이터(p31)

기본적인 데이터 퍼시스턴스 지원은 핵심 스프링 프레임워크에 포함되어 있지만, 스프링 데이터는 이외에 놀랄 만한 기능을 제공한다. 즉, 간단한 **자바 인터페이스로 애플리케이션의 데이터 리퍼지터리를 정의** 할 수 있다. 이때 데이터를 저장하고 읽는 메서드는 작명 규칙을 사용하여 정의한다.

게다가 스프링 데이터는 서로 다른 종류의 데이터베이스와 함께 사용할 수 있다. 예를 들어 관계형 데이터베이스인 JPA, 문서형 데이터베이스인 Mongo, 그래프형 데이터베이스인 Neo4j 등이다. 

## 1.4.4 스프링 시큐리티(p32)

스프링 시큐리티는 **인증**(authentication), **허가**(authorization), **API 보안** 을 포함하는 폭넓은 범위의 애플리케이션 보안 요구를 다룬다. 스프링만의 강력한 보안 프레임워크인 셈이다.

## 1.4.5 스프링 통합과 배치(p32)

대부분의 애플리케이션은 다른 애플리케이션 또는 같은 애플리케이션의 서로 다른 컴포넌트를 통합(integration)할 필요가 생긴다. 스프링 통합과 스프링 배치(batch)는 스프링 기반 애플리케이션의 통합 패턴을 구현한다.

스프링 통합은 데이터가 사용 가능한 즉시 처리되는 **실시간 통합** 을 한다. 반면 스프링 배치에서는 **다량의 데이터가 처리되는 시점**을 트리거(대기 시간을 기준으로 하는 트리거)가 알려줄 때 데이터가 수집 처리되는 배치 통합을 처리한다.

## 1.4.6 스프링 클라우드(p32)

최근에는 애플리케이션을 거대한 하나의 단일체로 개발하는 대신 **마이크로서비스** 라는 여러 개의 개별적인 단위들로 개발하는 추세이다. 이를 위해 클라우드 애플리케이션을 개발하기 위한 프로젝트들의 모음인 스프링 클라우드를 사용한다. 본서에서는 핵심적인 내용 일부를 다루었고, 자세한 내용을 알기 원한다면 <스프링 마이크로서비스 코딩 공작소(Spring Microservices in Action)>(존 카넬 저, 정성권 역, 길벗)을 참고하자.   

## 1장 요약(p33)

- 웹 애플리케이션 생성, 데이터베이스 사용, 애플리케이션 보안, 마이크로서비스 등에서 개발자의 노력을 덜어주는 것이 스프링의 목표이다.
- 스프링 부트는 손쉬운 의존성 관리, 자동-구성, 런타임 시의 애플리케이션 내부 작동 파악을 스프링에서 할 수 있도록 도와준다.
- 스프링 애플리케이션은 스프링 Initializr를 사용하여 초기 설정을 할 수 있다. 스프링 Initializr는 웹을 기반으로 하고 대부분의 자바 개발 환경을 지원한다.
- **빈**(bean)이라고 하는 컴포넌트는 스프링 애플리케이션 컨텍스트에서 **자바나 XML**로 선언할 수 있다. 그리고 **컴포넌트 탐색**으로 찾거나 **스프링 부트 자동-구성**에서 자동으로 구성할 수 있다.

## 2.1 정보 보여주기(p35~36)

> 본서에서 작업하는 타코 클라우드는 온라인으로 타코를 주문할 수 있는 애플리케이션이다. 

그리고 한걸음 더 나아가 타코 클라우드에서는 풍부한 식자재(ingredient)를 보여주는 팔레트를 사용하여 고객이 자신만의 커스텀 타코를 디자인할 수 있게 하고자 한다.

이를 위해 타코 클라우드 웹 애플리케이션은 고객에게 식자재를 보여주고 이를 선택할 수 있는 페이지를 가지고 있어야 한다. 식자재의 내역은 선택 시마다 수시로 변경될 수 있다. 따라서 HTML 페이지에 하드코딩하면 안 되며, 사용 가능한 식자재의 내역을 데이터베이스로부터 가져와 고객이 볼 수 있도록 해당 페이지에 전달해야 한다.

이러한 타코 디자인 페이지를 지원하기 위해 다음의 컴포넌트들을 생성할 것이다.

- 타코 식자재의 속성을 정의하는 도메인(domain) 클래스
- 식자재 정보를 가져와서 뷰에 전달하는 스프링 MVC 컨트롤러 클래스
- 식자재의 내역을 사용자의 브라우저에 보여주는 뷰 템플릿

우선 도메인에 대해 살펴볼 것이다. 이는 웹 컴포넌트를 개발할 수 있는 기초를 다지게 될 것이다.

## 2.1.1 도메인 설정하기(p36~41)

애플리케이션의 도메인은 **해당 애플리케이션의 이해에 필요한 개념을 다루는 영역** 이다. 타코 클라우드 애플리케이션의 도메인에는 다음과 같은 객체가 포함된다. 

- **고객**
- 고객이 선택한  **타코 디자인**
- 디자인을 구성하는 **식자재**
- 고객의 타코 **주문**

이중 우선 타코 식자재에 초점을 맞추기로 하자. 각 식자재는 개개의 이름과 타입(고기류, 치즈류, 소스류 등)을 갖는다.  또한 각 식자재는 쉽고 분명하게 참조할 수 있는 ID를 가진다. 타코 식자재를 정의하는 도메인 객체를 여기에서는 Ingredient 클래스로 정의하며 코드는 다음과 같다.

```java
package tacos;

import lombok.Data;
import lombok.RequiredArgsConstructor;

@Data
@RequiredArgsConstructor
public class Ingredient {
    
    private final String id;
    private final String name;
    private final Type type;
    
    public static enum Type {
        WRAP, PROTEIN, WEGGIES, CHEESE, SAUCE
    }
}
```

위의 Ingredient 클래스에서 특이한 점은 

- final 속성을 초기화하는 생성자가 없다는 점
- 속성들의 게터(getter)와 세터(setter) 메서드가 없다는 점
- equals(), hashCode(), toString() 등의 메서드를 정의하지 않았다는 점이다.

**Lombok**이라는 라이브러리는 특정 애노테이션을 통해 **런타임**에 이런 메서드들을 자동으로 생성해준다. 여기에서는 @Data 애노테이션을 지정하여 final 속성을 초기화하는 생성자와 속성들의 게터와 세터 메서드 등을 생성하도록 Lombok에게 알려주고 있다. (다만 Lombok을 사용하려면 프로젝트에 의존성을 먼저 추가해야 한다.)

이번에는 아래와 같이 Taco 클래스를 추가하자. 평범한 자바 도메인 객체로 두 개의 속성을 가지며, @Data 애노테이션을 지정해주었다.

```java
package tacos;

import java.util.List;
import lombok.Data;

@Data
public class Taco {
    private String name;
    private List<String> ingredients;
}
```

## 2.1.2 컨트롤러 클래스 생성하기(p41~44)

컨트롤러는 스프링 MVC 프레임워크의 중심적인 역할을 한다. HTTP 요청을 처리하고, 브라우저에 보여줄 HTML을 뷰에 요청하거나, REST 형태의 응답 Body에 데이터를 전달한다. 우선 이 장에서는 웹 브라우저의 콘텐츠를 생성하기 위해 뷰를 사용하는 컨트롤러에 초점을 둔다.  

타코 클라우드 애플리케이션의 경우 다음 기능을 수행하는 컨트롤러가 필요하다.

-  `/design` 요청 경로로 들어오는 HTTP GET 요청을 처리한다.
-  식자재의 내역을 생성한다.
-  식자재 데이터의 HTML 작성을 뷰 템플릿에 요청하고, 작성된 HTML을 웹 브라우저에 전송한다.

이런 요구사항을 처리하는 DesignTacoController 클래스는 다음과 같다.

```java
package tacos.web;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collections;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.extern.slf4j.Slf4j;
import tacos.Taco;
import tacos.Ingredient;
import tacos.Ingredient.Type;

@Slf4j
@Controller
@RequestMapping("/design")
public class DesignTacoController {
    
    @GetMapping
    public String showDesignForm(Model model) {
        List<Ingredient> ingredients = Arrays.asList(
        	new Ingredient("FLTO", "Flour Tortilla", Type.WRAP),
            new Ingredient("COTO", "Corn Tortilla", Type.WRAP),
            new Ingredient("GRBF", "Ground Beef", Type.PROTEIN),
            new Ingredient("CARN", "Carnitas", Type.PROTEIN),
            new Ingredient("TMTO", "Diced Tomatoes", Type.VEGGIES),
            new Ingredient("LETC", "Lettuce", Type.VEGGIES),
            new Ingredient("CHED", "Cheddar", Type.CHEESE),
            new Ingredient("JACK", "Monterrey Jack", Type.CHEESE),
            new Ingredient("SLSA", "Salsa", Type.SAUCE),
            new Ingredient("SRCR", "Sour Cream", Type.SAUCE)
        );
        
        Type[] types = Ingredient.Type.values();
        for (Type type : types) {
            models.addAttribute(type.toString().toLowerCase()),
            	filterByType(ingredients, type));
        }
        
        model.addAttribute("taco", new Taco());
        
        return "design";
    }
    
    private List<Ingredient> filterByType(
    	List<Ingredient> ingredients, Type type) {
      return ingredients.stream()
          			.filter(x -> x.getType().equals(type))
          			.collect(Collectors.toList());
    }
}
```

DesignTacoController 클래스에서 먼저 주목할 부분은 클래스 수준에 적용된 애노테이션들이다. 우선 **@Slf4j** 는 컴파일 시 Lombok에 제공되며, 이 클래스에 **자동으로 SLF4J(자바에 사용하는 Simple Logging Facade) Logger를 생성** 한다. 이 애노테이션은 다음 코드를 추가한 것과 같은 효과를 가진다.

```java
private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(DesignTacoController.class);
```

그 다음 살펴볼 애노테이션은 @Controller이다. 이 애노테이션은 DesignTacoController 클래스가 컨트롤러로 식별되게 하며, 컴포넌트 검색을 해야 한다는 것을 나타낸다. 따라서 스프링이 DesignTacoController 클래스를 찾은 후 스프링 애플리케이션 컨텍스트의 빈으로 이 클래스의 인스턴스를 자동 생성한다.

또 다른 애노테이션으로 @RequestMapping이 있다. 이 애노테이션이 클래스 수준으로 적용될 때는 해당 컨트롤러가 처리하는 요청의 종류를 나타낸다. 여기에서는 DesignTacoController에서 /design으로 시작하는 경로의 요청을 처리함을 나타낸다.

## GET 요청 처리하기(p44~45)

 @GetMapping 애노테이션은 /design에 HTTP GET 요청이 수신될 때 그 요청을 처리하기 위해 showDesignForm() 메서드가 호출됨을 나타낸다. 이 애노테이션은 스프링 4.3에서 소개되었다. 그 이전에는 아래와 같이 메서드 수준의 @RequestMapping 애노테이션을 사용하였다. 

```java
@RequestMapping(method=RequestMethod.GET)
```

스프링 MVC에서 사용할 수 있는 요청-대응 애노테이션은 다음의 표와 같다.

| 애노테이션      | 설명                         |
| --------------- | ---------------------------- |
| @RequestMapping | 다목적 요청을 처리한다.      |
| @GetMapping     | HTTP GET 요청을 처리한다.    |
| @PostMapping    | HTTP POST 요청을 처리한다.   |
| @PutMappping    | HTTP PUT 요청을 처리한다.    |
| @DeleteMapping  | HTTP DELETE 요청을 처리한다. |
| @PatchMapping   | HTTP PATCH 요청을 처리한다.  |

컨트롤러 메서드에 대해 요청-대응 애노테이션을 선언할 때는 **가급적 특화된 것을 사용** 하는 것이 좋다. 즉, 경로(또는 클래스 수준의 @RequestMapping에서 경로를 상속받음)를 지정하는 애노테이션과 처리하려는 특정 HTTP 요청을 지정하는 애노테이션 모두 각각 선언한다는 의미이다. 위의 요청-대응 애노테이션들은 @RequestMapping과 같은 속성을 가지므로 @RequestMapping을 사용했던 메서드에도 사용할 수 있다.

이번에는 showDesignForm() 메서드 블록을 살펴보자. 식자재를 나타내는 Ingredient 객체를 저장하는 List를 생성한다. 지금은 Ingredient 객체들을 직접 코드에서 추가하지만 이후 데이터베이스로부터 데이터를 가져와 저장할 것이다. 

그 다음 식자재의 유형(고기, 치즈, 소스 등)을 List에서 필터링(filterByType 메서드)한 후 showDesignForm()의 인자 Model 객체의 속성으로 추가한다. Model은 컨트롤러와 뷰 사이에서 데이터를 운반하는 객체이다. 궁극적으로 Model 객체의 속성에 있는 데이터는 뷰가 알 수 있는 서블릿(servlet) 요청 속성들로 복사된다. showDesignForm() 메서드는 마지막에 "design"을 반혼한다. 이는 모델 데이터를 브라우저에 나타내는 데 사용될 뷰의 논리적인 이름이다.

지금까지 정의한 코드에 따르면 /design 경로에 접속할 때 DesignTacoController의 showDesignFrom() 메서드가 실행된다. 그리고 뷰에 요청이 전달되기 전에 List에 저장된 식자재 데이터를 모델 객체(Model)에 넣을 것이다. 그러나 아직 뷰를 정의하지 않았기 때문에 웹 브라우저는 HTTP 404 (Not Found) 에러를 보여줄 것이다. 이 문제를 해결하기 위해 지금부터 뷰를 알아보기로 하자. 데이터가 HTML 안에 작성되어 사용자의 웹 브라우저에 나타나게 하는 것이 뷰의 역할이다. 

## 2.1.3 뷰 디자인하기(p45~51)

스프링은 뷰를 정의하는 여러 가지 방법을 제공하는데,  JSP(JavaServer Pages), Thymeleaf, FreeMarker, Mustache, 그루비(Groovy) 기반 템플릿 등이다. 여기에서는 이중 Thymeleaf를 사용할 것이고, Thymeleaf를 사용하기 위해 프로젝트의 빌드 구성 파일(pom.xml)에 또 다른 의존성을 추가해야 한다. (본서에서는 1장에서 이미 추가하였다.)  

의존성을 추가하면 스프링 부트의 자동-구성에서 런타임에 classpath의 Thymeleaf를 찾아 빈(스프링 MVC에 Thymeleaf 뷰를 지원하는)을 자동으로 생성한다.

Thymeleaf와 같은 뷰 라이브러리들은 어떤 웹 프레임워크와도 사용 가능하도록 설계되었다. 따라서 스프링으 ㅣ추상화 모델을 알지 못하며, 컨트롤러가 데이터를 넣는 Model 대신 서블릿 요청 속성을 사용한다. 뷰에게 요청을 전달하기에 앞서 스프링은 Thymeleaf와 이외의 다른 뷰 템플릿이 사용하는 요청 속성에 모델 데이터를 복사한다.

Thymeleaf 템플릿은 요청 데이터를 나타내는 요소 속성을 추가로 가지는 HTML이다. 예를 들어 키(key)가 "message"인 요청 속성이 있고, 이를 Thymeleaf를 사용하여 HTML `<p>` 태그로 나타내고자 한다면 다음과 같이 Thymeleaf 템플릿에 작성해야 한다.

```html
<p th:text="${message}">placeholder message</p>
```

이 경우 템플릿이 HTML로 표현될 때 `<p>` 요소의 몸체는 키가 "message"인 서블릿 요청 속성의 값으로 교체된다. th:text는 교체를 수행하는 Thymeleaf 네임스페이스(namespace) 속성이다. ${} 연산자는 요청 속성(여기에서는 "message")의 값을 사용하라는 것을 알려준다.

Thymeleaf는 th:each라는 속성 또한 제공한다. 이는 List와 같은 컬렉션을 반복 처리하며, 컬렉션의 각 요소를 하나씩 HTML로 나타낸다. 따라서 List에 저장된 타코 식자재를 모델 데이터로부터 뷰에 보여ㅇ줄 때 편리하다. 예를 들어 토르티아(tortilla)를 "wrap" 유형의 식자재 내역을 나타낼 때 다음과 같이 HTML을 사용할 수 있다.

```html
<h3>Designate your wrap:</h3>
<div th:each="ingredient : ${wrap}">
    <input name="ingredients" type="checkbox" th:value="${ingredient.id}" />
    <span th:text="${ingredient.name}">INGREDIENT</span><br/>
</div>
```

실제 모델 데이터를 사용했을 때 생성되는 `<div>` 중 하나를 예를 들면 다음과 같다.

```html
<div>
    <input name="ingredients" type="checkbox" value="FLTO" />
    <span>Flour Tortilla</span><br/>
</div>
```

 이제 design.html에 정의할 전체 템플릿 내용을 알아보자. 코드는 다음과 같다.

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="EUC-KR">
<title>Taco Cloud</title>
<link rel="stylesheet" th:href="@{/styles.css}" />
</head>

<body>
	<h1>Design your taco!</h1>
	<img th:src="@{/images/TacoCloud.png}" />
	
	<form method="POST" th:object="${taco}">
		<span class="validationError"
			th:if="${#fields.hasErrors('ingredients')}"
			th:errors="*{ingredients}">Ingredient Error</span>
		
		<div class="grid">
			<div class="ingredient-group" id="wraps">
				<h3>Designate your wrap:</h3>
				
				<div th:each="ingredient : ${wrap}">
					<input name="ingredients" type="checkbox"
						th:value="${ingredient.id}" /> 
					<span th:text="${ingredient.name}">INGREDIENT</span><br />
				</div>
			</div>
			
			<div class="ingredient-group" id="proteins">
				<h3>Pick your protein:</h3>
				<div th:each="ingredient : ${protein}">
					<input name="ingredients" type="checkbox"
						th:value="${ingredient.id}" /> 
					<span th:text="${ingredient.name}">INGREDIENT</span><br />
				</div>
			</div>
			
			<div class="ingredient-group" id="cheeses">
				<h3>Choose your cheese:</h3>
				<div th:each="ingredient : ${cheese}">
					<input name="ingredients" type="checkbox"
						th:value="${ingredient.id}" /> 
					<span th:text="${ingredient.name}">INGREDIENT</span><br />
				</div>
			</div>
			
			<div class="ingredient-group" id="veggies">
				<h3>Determine your veggies:</h3>
				<div th:each="ingredient : ${veggies}">
					<input name="ingredients" type="checkbox"
						th:value="${ingredient.id}" /> 
					<span th:text="${ingredient.name}">INGREDIENT</span><br />
				</div>
			</div>
			
			<div class="ingredient-group" id="sauces">
				<h3>Select your sauce:</h3>
				<div th:each="ingredient : ${sauce}">
					<input name="ingredients" type="checkbox"
						th:value="${ingredient.id}" /> 
					<span th:text="${ingredient.name}">INGREDIENT</span><br />
				</div>
			</div>
		</div>
		
		<div>
			<h3>Name your taco creation:</h3>
			<input type="text" th:field="*{name}" />  
			<span
				class="validationError" th:if="${#fields.hasErrors('name')}"
				th:errors="*{name}">Name Error</span> 
			<br />
			<button>Submit your taco</button>
		</div>
	</form>
</body>
</html>
```

각 유형의 식자재마다 `<div>` 코드가 반복되어 있고,  마지막 `<div>` 부분에서는 사용자 자신이 생성한 타코의 이름을 지정할 수 있는 필드와 Submit 버튼을 포함하고 있다.

`<body> ` 태그 맨 앞에 있는 타코 클라우드 로고 이미지와 `<head>` 태그 안에 `<link>` 스타일시트 참조도 주목할 필요가 있다. 두 가지에서 모두 Thymeleaf의 @{} 연산자가 사용되었다. 이로써 참조되는 정적 콘텐츠(로고 이미지와 스타일시트)들의 위치(컨텍스트 상대 경로)를 알 수 있다. 스프링 부트 애플리케이션의 **정적 콘텐츠**는 **classpath의 루트 밑에 있는 /static 디렉터리** 에 위치한다. 

스타일시트 내용은 각 유형의 식자재들을 두 개의 컬럼으로 보여주는 스타일만을 포함한다. 자세한 내용은 [이곳](https://github.com/Jpub/SpringInAction5/blob/master/Ch02/taco-cloud/src/main/resources/static/styles.css)을 참조하자.

이제 애플리케이션을 실행하여 뷰와 컨트롤러의 연결을 확인해볼 수 있다. 스프링 부트 애플리케이션을 실행하는 방법은 여러가지다. 예를 들어 애플리케이션을 실행 가능한 JAR 파일로 빌드한 후 java -jar로 JAR를 실행하거나, 또는 mvn spring-boot:run을 사용하여 실행할 수 있다. 또한 IDE의 시작 버튼을 눌러 애플리케이션을 실행할 수 있다. 

실행을 한 이후 웹 브라우저에서 http://localhost:8080/design에 접속해보자. 아까는 404 (Not Found) 에러가 나왔지만 이제는 그럴듯한 페이지가 보일 것이다. 이 /design 페이지에서 사용자는 자신만의 타코를 만들기 위해 원하는 식자재를 선택할 수 있다. 하지만 만약 Submit your taco 버튼을 클릭하면 어떻게 될까?

사용자가 Submit 버튼을 클릭하면 또 다른 요청이 발생한다. 하지만 우리는 아직 DesignTacoController가 이 요청을 어떻게 처리할 것인지 정의하지 않았다. 다음 내용에서는 이 문제를 해결하기 위해 폼 제출이 있을 때 이를 처리하는 컨트롤러 코드를 추가할 것이다.

## 2.2 폼 제출 처리하기(p51~57)

뷰(design.html)의 `<form>` 태그를 다시 보면 method 속성이 POST로 설정되어 있지만 `<form>` 태그에는 action 속성이 선언되지 않았다. 이 경우 폼이 제출되면 브라우저가 폼의 모든 데이터를 모아서 GET 요청과 같은 경로(/design)로 HTTP POST 요청을 서버에 전송한다. 따라서 이 요청을 처리하는 컨트롤러의 메서드가 있어야 한다. 다음의 코드는 /design 경로의 POST 요청을 처리하는 DesignTacoContorller의 새로운 메서드를 포함하고 있다.

```java
...
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.PostMapping;
...

public class DesignTacoController {
  ...
  @PostMapping
  public String processDesign(Taco design) {
    // 이 지점에서 타코 디자인(선택된 식자재 내역)을 저장한다.
    // 이 작업은 3장에서 다룰 것이다.
    log.info("Processing design: " + design);
    
    return "redirect:/orders/current";
  }
}
```

타코 디자인 폼이 제출될 때 이 폼의 필드는 processDesign()의 인자로 전달되는 Taco 객체의 속성과 바인딩된다. 따라서 processDesign() 메서드에서는 Taco 객체를 사용하여 어떤 것이든 원하는 처리를 할 수 있다.

타코 디자인 폼에는 checkbox 요소들이 여러 개 존재한다. 이들 요소는 모두 ingredients 라는 이름을 가지고 있고, 텍스트 입력 요소의 이름을 name으로 한다. 이 필드들은 Taco 클래스의 ingredients 및 name 속성 값과 바인딩된다.

폼의 Name 필드는 텍스트 값을 가지기 때문에 Taco의 name 속성은 String 타입을 가진다. 식자재를 나타내는 checkbox들도 텍스트 값을 가지는데, 이는 0개 또는 여러 개가 선택될 수 있기 때문에 이와 바인딩되는 Taco 클래스의 ingredients 속성은 선택된 식자재들의 id를 저장하기 위해 List<String> 타입이어야 한다.

showDesignFrom() 메서드처럼 processDesign()도 String 값을 반환하며, 이 값도 사용자에게 보여주는 뷰를 나타낸다. 그러나 processDesign()에서 반환 값은 리디렉션(redirection, 변경된 경로로 재접속) 뷰를 나타내는 "redirect:"가 제일 앞에 붙는다. 즉, processDesign()의 실행이 끝난 후 사용자의 브라우저는 /orders/current 상대 경로로 재접속할 것이다.

이로써 사용자는 자신이 디자인한 타코를 받기 위해 주문을 처리하는 폼으로 접속할 수 있다. 하지만 /orders/current 경로의 요청을 처리할 컨트롤러가 아직 없다. 다음의 코드를 작성하여 해당 컨트롤러를 만들 수 있다.

```java
package tacos.web;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import lombok.extern.slf4j.Slf4j;
import tacos.Order;

@Slf4j
@Controller
@RequestMapping("/orders")
public class OrderController {
  
  @GetMapping("/current")
  public String orderForm(Model model) {
    model.addAttribute("order", new Order());
    return "orderForm";
  }
}
```

참고로 아직 Order 클래스는 정의하지 않았다. 이는 추후에 작성하기로 한다.

orderForm 뷰는 orderForm.html 이라는 이름의 Thymeleaf 템플릿에 의해 제공된다. 코드는 아래와 같다.

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml"
	  xmlns:th="http://www.thymeleaf.org">
  <head>
	<meta charset="EUC-KR">
	<title>Taco Cloud</title>
	<link rel="stylesheet" th:href="@{/styles.css}" />
  </head>
  <body>
	
	<form method="POST" th:action="@{/orders}" th:object="${order}">
		<h1>Order your taco creations!</h1>
		
		<img th:src="@{/images/TacoCloud.png}" /> <a th:href="@{/design}"
			id="another">Design another taco</a><br />
		
		<div th:if="${#fields.hasErrors()}">
			<span class="validationError"> Please correct the problems
				below and resubmit. </span>
		</div>
		
		<h3>Deliver my taco masterpieces to...</h3>
		
		<label for=" deliveryName">Name: </label> 
		<input type="text" th:field="*{deliveryName}" /> 
		<span class="validationError"
			th:if="${#fields.hasErrors('deliveryName')}"
			th:errors="*{deliveryName}">Name Error</span>
		<br /> 
			
		<label for="deliveryStreet">Street address: </label> 
		<input type="text" th:field="*{deliveryStreet}" /> 
		<span class="validationError"
			th:if="${#fields.hasErrors('deliveryStreet')}"
			th:errors="*{deliveryStreet}">Street Error</span>
		<br /> 
		
		<label for="deliveryCity">City: </label> 
		<input type="text" th:field="*{deliveryCity}" /> 
		<span class="validationError"
			th:if="${#fields.hasErrors('deliveryCity')}"
			th:errors="*{deliveryCity}">City Error</span>
		<br /> 
		
		<label for="deliveryState">State: </label> 
		<input type="text" th:field="*{deliveryState}" />
		<span class="validationError"
			th:if="${#fields.hasErrors('deliveryState')}"
			th:errors="*{deliveryState}">State Error</span>
		<br /> 
		
		<label for="deliveryZip">Zip code: </label> 
		<input type="text" th:field="*{deliveryZip}" /> 
		<span class="validationError"
			th:if="${#fields.hasErrors('deliveryZip')}"
			th:errors="*{deliveryZip}">Zip Error</span>
		<br />
		
		<h3>Here's how I'll pay...</h3>
		<label for="ccNumber">Credit Card #: </label> 
		<input type="text" th:field="*{ccNumber}" /> 
		<span class="validationError"
			th:if="${#fields.hasErrors('ccNumber')}"
			th:errors="*{ccNumber}">CC Num Error</span>
		<br /> 
		
		<label for="ccExpiration">Expiration: </label> 
		<input type="text" th:field="*{ccExpiration}" /> 
		<span class="validationError"
			th:if="${#fields.hasErrors('ccExpiration')}"
			th:errors="*{ccExpiration}">CC Num Error</span>
		<br /> 
		
		<label for="ccCVV">CVV: </label> 
		<input type="text" th:field="*{ccCVV}" /> 
		<span class="validationError"
			th:if="${#fields.hasErrors('ccCVV')}"
			th:errors="*{ccCVV}">CC Num Error</span>
		<br />
		
		<input type="submit" value="Submit order" />
	</form>
  </body>
</html>
```

위의 `<form>` 태그에서 action 값이 /orders 경로로 지정되어 있다(Thymeleaf의 @{...} 연산자를 사용하여 컨텍스트 상대 경로인 /orders를 지정하였다). 따라서 /order 경로의 POST 요청을 처리하는 또 다른 메서드를 OrderController 클래스에 추가해야 한다. 추가할 코드는 다음과 같다.

```java
...
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.PostMapping;
...
  
public class OrderController {
  ...
  @PostMapping
  public String processOrder(Order order) {
    log.info("Order submitted: " + order);
    return "redirect:/";
  }
}
```

제출된 주문을 처리하기 위해 processOrder() 메서드가 호출될 때는 제출된 폼 필드와 바인딩된 속성을 갖는 Order 객체가 인자로 전달된다. Order는 주문 정보를 갖는 간단한 클래스로 다음과 같이 정의한다.

```java
package tacos;

import lombok.Data;

@Data
public class Order {
  
  private String deliveryName;
  private String deliveryStreet;
  private String deliveryCity;
  private String deliveryState;
  private String deliveryZip;
  private String ccNumber;
  private String ccExpiration;
  private String ccCVV;
}
```

이제 OrderController와 주문 폼 뷰의 작성이 끝났으므로 애플리케이션을 실행할 수 있다. 웹 브라우저로 http://localhost:8080/design에 접속하자. 그리고 타코 식자재 중 몇 가지를 선택한 후 Submit your taco 버튼을 클릭하면 방금 작성한 주문 폼을 볼 수 있다. 

폼의 일부 필드에 아무 값이나 입력하고 Submit order 버튼을 누르면 주문 처리가 끝나고 타코 홈페이지로 이동할 것이다.주문 정보를 보기 위해 애플리케이션 로그를 살펴보면 값은 다르지만 다음과 같은 형태의 로그를 확인할 수 있다.

```bash
2021-08-15 19:36:52.095 INFO 10800 --- [nio-8080-exec-3] tacos.web.OrderController: Order submitted: Order(deliveryName=이보람, deliveryStreet=배송거리, deliveryCity=배송도시, deliveryState=해당없음, deliveryZip=배송우편번호, ccName=cc번호, ccExpiration=cc만료일자, ccCVV=ccCVV)
```

위의 로그 항목을 살펴보면, processOrder() 메서드가 실행되어 폼 제출이 정상적으로 처리되었지만, 잘못된 정보의 입력을 허용한다는 것을 알 수 있다. 따라서 우리가 필요한 정보에 맞게 데이터를 검사해야 한다.

## 2.3 폼 입력 유효성 검사하기(p57~65)

타코 디자인 페이지에서 사용자가 새로운 타코를 디자인할 때 아무런 식자재를 선택하지 않거나, 생성되는 타코의 이름을 입력하지 않거나 다섯 글자보다 적게 입력하면 어떻게 될까? 또는 사용자가 타코의 주문을 제출할 대 필수 입력 필드에 데이터를 입력하지 않을 경우나 신용 카드 필드에 유효하지 않은 신용 카드 번호를 입력하면 어떻게 될까? 우리는 아직 어떤 필드에도 유효성 검사를 추가하지 않았으므로 모든 요청이 문제 없이 다음 작업을 진행할 것이다. 하지만 우리가 실제 주문을 받는다고 가정하면 위의 경우들은 문제가 될 수 있다.

폼의 유효성 검사를 위해 스프링은 **자바의 빈 유효성 검사** (Bean validation API)를 지원한다. 이를 통해 애플리케이션 각각의 메서드에 지저분한 if/else 문을 추가하지 않고 유효성 검사 규칙을 쉽게 선언할 수 있다. 긜고 스프링 부트를 사용하면 **유효성 검사 라이브러리** 를 프로젝트에 쉽게 추가할 수 있다. 유효성 검사 API와 이 API를 구현한 Hibernate 컴포넌트는 스프링 부트의 웹 스타터 의존성으로 자동 추가되기 때문이다. 

스프링 MVC에 유효성 검사를 적용하려면 다음과 같이 해야 한다.

- 유효성을 검사할 클래스(여기에서는 Taco와 Order)에 검사 규칙을 선언한다.
- 유효성 검사를 해야 하는 컨트롤러 메서드에 검사를 수행한다는 것을 지정한다. 여기에서 DesignTacoController의 processDesign() 메서드와 OrderController의 processOrder() 메서드가 해당된다.
- 검사 에러를 보여주도록 폼 뷰를 수정한다.

유효성 검사 API는 몇 가지 애노테이션을 제공한다. 이 애노테이션들은 검사 규칙을 선언하기 위해 **도메인 객체의 속성에 지정** 할 수 있다. 유효성 검사 API를 구현한 Hibernate 컴포넌트에는 더 많은 유효성 검사 애노테이션이 추가되었다. 다음부터 Taco와 Order의 유효성 검사를 하는 애노테이션의 적용법을 알아보기로 하자.

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
package tacos.data;import org.springframework.data.repository.CrudRepository;import tacos.Ingredient;public interface IngredientRepository extends CrudRepository<Ingredient, String> {        // Iterable<Ingredient> findAll();    // Ingredient findById(String id);    // Ingredient save(Ingredient ingredient);}
```

CrudRepository 인터페이스의 첫 번째 매개변수는 리퍼지터리에 저장되는 개체 타입이고, 두 번째 매개변수는 개체 id 속성의 타입이다. 

인터페이스와 이에 정의된 메소드의 구현을 위해 클래스 구현체를 작성해야 한다고 생각할 수 있다. 하지만 그럴 필요가 없다. 애플리케이션이 시작될 때 스프링 데이터 JPA는 각 인터페이스 구현체(클래스 등)를 자동으로 생성해주기 때문이다. 즉 스프링 데이터 JPA를 사용하면 리퍼지터리만 작성하면 된다. 그리고 JDBC 기반의 구현에서 하듯이 리퍼지터리를 필요한 곳에 주입해주면 된다. 	

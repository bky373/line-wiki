# Spring in Action

## 책 정보

- 저자 : 크레이그 월즈
- 옮긴이 : 심채철
- 출판사 : 제이펍
- 출간일 : 2020년 05월 14일
- 에디션 : 5th Edition
- 관련링크 :  [실습용 소스 코드](https://github.com/Jpub/SpringInAction5)

# Chapter 분류

- [Chapter 01. 스프링 시작하기](https://github.com/bky373/line-wiki/blob/main/Spring_In_Action/CH_01_%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0.md#Chapter-01-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)

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

## 2.3.1 유효성 검사 규칙 선언하기(p58~60)

Taco 클래스는 name 속성 값이 없거나 null인지 확인하며, 최소 하나 이상의 식자재 항목을 선택했는지 확인할 것이다. 다음 코드에서는 이와 같은 유효성 검사 규칙을 선언하기 위해 @NotNull과 @Size를 사용하였다.

```java
package tacos;

import java.util.List;

import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

import lombok.Data;

@Data
public class Taco {
  
  @NotNull
  @Size(min=5, message="Name must be at least 5 characters long")
  private String name;
  
  @Size(min=1, message="You must choose at least 1 ingredient")
  private List<String> ingredients;
}
```

name 속성 값이 null이 아니어야 한다는 규칙과 함께 최소 5개 문자로 이루어져야 한다는 규칙을 선언하였다.

이번에는 타코 주문의 유효성 검사를 하기 위해 Order 클래스에 애노테이션을 추가할 것이다. 배달 주소 속성들(street, city, state, zip)은 사용자가 입력하지 않은 필드가 있는지 확인하면 되므로 자바 빈 유효성 검사 API의 @NotBlank 애노테이션을 사용할 것이다.

그러나 대금 지불에 관한 필드의 경우 보다 엄격한 유효성 검사가 필요하다. ccNumber 속성 값은 null이 아니어야 하며, 입력 값이 유효한 신용카드 번호인지도 확인해야 한다. ccExpiration 속성은 MM/YY(두 자리 수의 월과 연도) 형식의 값이어야 한다. ccCVV 속성의 값은 세 자리 숫자이어야 한다. 이와 같은 유효성 검사를 하기 위해 다른 자바 빈 유효성 검사 API 애노테이션과 Hibernate Validator의 또 다른 애노테이션을 사용해야 한다. 이제 Order 클래스에 유효성 검사 규칙을 선언하자.

```java
package tacos;

import javax.validation.constraints.Digits;
import javax.validation.constraints.Pattern;
import javax.validation.constraints.NotBlank;
import org.hibernate.validator.constraints.CreditCardNumber;

import lombok.Data;

@Data
public class Order {
  
  @NotBlank(message="Name is required")
  private String deliveryName;
  
  @NotBlank(message="Street is required")
  private String deliveryStreet;
  
  @NotBlank(message="City is required")
  private String deliveryCity;
    
  @NotBlank(message="State is required")
  private String deliveryState;
  
  @NotBlank(message="Zip code is required")
  private String deliveryZip;
  
  @CreditCardNumber(message="Not a valid credit card number")
  private String ccNumber;
  
  @Pattern(regexp="^(0[1-9]|1[0-2])([\\/])([1-9][0-9])$",
           message="Must be formatted MM/YY")
  private String ccExpiration;
  
  @Digits(integer=3, fraction=0, message="Invalid CVV")
  private String ccCVV;
}
```

위의 코드를 보면 ccNumber 속성에는  @CreditCardNumber가 지정되어 있다. 이 애노테이션은 속성 값이 Luhn(룬) 알고리즘 검사를 합격한 유효한 신용 카드 번호인지 확인한다. 단 입력된 카드 번호가 실제로 존재하는 것인지, 또는 대금 지불에 사용될 수 있는 것인지는 검사하지 못한다.(이를 위해서는 실시간으로 금융망과 연동되어야 한다.)

ccExpiration 속성 값은 MM/YY 형식인지를 검사해야 하지만 이에 사용할 수 있는 애노테이션이 없다. 따라서 여기에서는 @Pattern 애노테이션에 정규 표현식을 지정하여 속성 값이 해당 형식을 따르는지 확인하였다.

마지막으 ccCVV 속성에서는 @Digits 애노테이션을 지정하여 입력 값이 정확히 세 자리 숫자인지 검사한다.

모든 유효성 검사 애노테이션은 message 속성을 가지고 있다. 사용자가 입력한 정보가 각 애노테이션의 유효성 규칙을 통과하지 못할 경우 message 속성에 정의된 값을 보여준다.

## 2.3.2 폼과 바인딩될 때 유효성 검사 수행하기(p60~62)

Taco와 Order의 유효성 검사 규칙 선언이 끝났으므로, 각 폼의 POST 요청이 관련 메서드에서 처리될 때 유효성 검사가 수행되도록 다음과 같이 컨트롤러를 수정해야 한다.

```java
...
import javax.validation.Valid;
import org.springframework.validation.Errors;
...
  
@PostMapping
public String processDesign(@Valid Taco design, Errors errors) {
  if (errors.hasErrors()) {
    return "design";
  }
  
  // 이 지점에서 타코 디자인(선택된 식자재 내역)을 저장한다.
  // 이 작업은 3장에서 진행한다.
  log.info("Processing design: " + design);
  return "redirect:/orders/current";
}
```

@Valid 애노테이션은, 제출된 폼 데이터와 Taco 객체가 바인딩된 후, processDesign() 메서드의 코드가 실행되기 전, 제출된 Taco 객체의 유효성 검사를 수행하라고 스프링 MVC에게 알려준다.

만약 검사시 에러가 발견되면 에러의 상세 내역이 Errors 객체에 저장되어 processDesign()으로 전달된다. 위의 코드에서 검사 에러가 있으면 Taco의 처리를 중지하고  "design" 뷰 이름을 반환하여 폼이 다시 보이게 한다.

한편, 제출된 Order의 유효성 검사를 위해 OrderController의 processOrder() 메서드를 변경할 것이다. 다음 코드를 참고하자.

```java
...
import javax.validation.Valid;
import org.springframework.validation.Errors;
...
  
@PostMapping
public String processOrder(@Valid Order order, Errors errors) {
  if (errors.hasErrors()) {
    return "orderForm";
  }
  
  log.info("Order submitted: " + order);
  return "redirect:/";
}
```

processDesign()과 processOrder() 메서드 모두 검사시 에러가 발견되면, 사용자가 입력 오류를 수정할 수 있도록 해당 요청이 폼 뷰에 다시 보내진다.

하지만 사용자는 자신이 무엇을 수정해야 하는지 아직 알지 못한다. 다음에서는 폼에서 어떤 에러가 발생했는지 사용자가 알 수 있도록 코드를 변경할 것이다.

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

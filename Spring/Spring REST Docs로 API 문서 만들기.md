# Spring REST Docs로 API 문서 만들기

스프링 애플리케이션을 사용할 때, API 문서를 만드는 프로세스를 간단히 알아보자.

## 1. 진행할 내용

- **먼저, 간단한 스프링 애플리케이션을 빌드한다.**
  - 애플리케이션은 몇 가지 **HTTP 엔드포인트**를 가지고 있으며, 이들은 API로 사용된다.
- **웹 레이어(Web Layer) 만 테스트한다.**
  - 테스트시에는 **JUnit**과 스프링의 **MockMvc** 를 사용한다.
  - 이후 **Spring REST Docs**를 추가하여 API 문서를 생성한다.

## 2. 요구 사항

- JDK 1.8 이상
- [Gradle 4+](http://www.gradle.org/downloads) or [Maven 3.2+](https://maven.apache.org/download.cgi)
- IDE

## 3. Spring Initializr로 시작하기

> [Download 또는 git clone](https://spring.io/guides/gs/testing-restdocs/#initial) 또는 IDE로 시작해도 되지만, 여기에서는 처음부터 시작한다.

- [Spring Initializr](https://start.spring.io)에서 프로젝트를 초기화한다. 여기에서 애플리케이션에 필요한 모든 종속성을 가져오고 대부분의 설정을 자동 수행한다.

- **Gradle** 또는 Maven 그리고 **Java**를 선택한다(여기에서는 Gradle을 선택한다).

  ![image](https://user-images.githubusercontent.com/87057868/135096249-7e5d27b5-95b4-4ede-9380-2bbf9bb4f2df.png)

- Artifact 이름을 `testingrestdocs`로 변경한다(사실 굳이 변경하지 않아도 된다). Artifact를 변경하면 Name은 자동으로 변경된다.

  ![image](https://user-images.githubusercontent.com/87057868/135194025-8127f311-a8f7-43b1-ab5e-d41a35bf62ab.png)

- Dependencies 항목에서 [ ADD DEPENDENCIES... ] 버튼을 클릭한 다음 **Spring Web** 을 검색, 선택한다.![image](https://user-images.githubusercontent.com/87057868/135096562-722281be-394e-43ff-bf31-faca45df288f.png)

- 페이지 하단의 [ GENERATE ] 버튼을 클릭하여 ZIP 파일을 다운로드한다.

- 다운로드 이후, ZIP 파일을 압축 해제하고 IDE에서 해당 폴더를 열어준다.

 ## 4. 간단한 애플리케이션 만들기

- 아래 위치에 `HomeController` 를 만든다.

```java
// src/main/java/com/example/testingrestdocs/HomeController.java

package com.example.testingrestdocs;

import java.util.Collections;
import java.util.Map;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HomeController {

  @GetMapping("/")
  public Map<String, Object> greeting() {
    return Collections.singletonMap("message", "Hello, World");
  }
}

```

## 5. 애플리케이션 실행하기

- Spring Initializr는 애플리케이션을 시작하기 위해 필요한 기본적인 애플리케이션 클래스를 생성해준다.

- `main()` 메서드는 스프링 부트의 SpringApplication.run() 메서드를 사용하여 애플리케이션을 시작한다.

- 이 애플리케이션은 xml 파일을 가지지 않으며, 순수 Java로 작성되어 있다. 스프링 부트가 애플리케이션의 인프라 구성을 책임지고 처리한다.

  ```java
  package com.example.testingrestdocs;
  
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  
  @SpringBootApplication
  public class TestingrestdocsApplication {
  
  	public static void main(String[] args) {
  		SpringApplication.run(TestingrestdocsApplication.class, args);
  	}
  
  }
  ```

- `main()` 메서드를 실행하면 다음과 같이 로깅 작업이 출력된다.

  ![image](https://user-images.githubusercontent.com/87057868/135209396-ef19710e-af38-472e-8bc6-73a6f27a1698.png)

## 6. 애플리케이션 테스트하기

- 애플리케이션이 실행 중이므로 이제 `http://localhost:8080`으로 접속했을 때 아래와 같은 화면이 로드된다. 

  ![image](https://user-images.githubusercontent.com/87057868/135195465-4b43426e-5e06-4732-b8f0-042f233f7156.png)

- 여기서 더 나아가, 변경사항이 있을 때 애플리케이션이 작동한다는 것을 확신할 수 있도록 테스트를 자동화하려고 한다.

- 또한, REST Docs를 이용해 테스트의 동적인 부분을 생성하고 HTTP 엔드포인트에 대한 문서를 작성해볼 것이다.

### 6-1. 빌드 구성: 종속성 추가

본격적인 작업에 앞서 Spring Test와 Spring REST Docs 종속성을 추가하자. 여기에서는 Gradle을 사용하므로, `build.gradle` 파일을 아래와 같이 변경하면 된다.

```java
plugins {
	id 'org.springframework.boot' version '2.5.2'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
	id 'org.asciidoctor.jvm.convert' version '3.3.0'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

asciidoctor {
	baseDirFollowsSourceDir()
	dependsOn test
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
  // tag::test[]
	testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
	// end::test[]
	testImplementation('org.springframework.boot:spring-boot-starter-test')
}

test {
	useJUnitPlatform()
}
```



### 6-2. 테스트 코드 작성

다음으로 새너티 체크를 진행한다. 새너티 체크(sanity check)는 주요 테스트 케이스를 작성하기에 앞서 진행하는 소프트웨어 안전성 검사를 말한다. 아래 코드는 새너티 체크를 위한 것이다.

```java
// src/test/java/com/example/testingrestdocs/TestingRestdocsApplicationTests.java

package com.example.testingrestdocs;

import org.junit.jupiter.api.Test;

import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
public class TestingRestdocsApplicationTests {

	@Test
	public void contextLoads() throws Exception {
	}
}

```

IDE 또는 커맨드라인에서 `./grablew test` 명령을 수행하여 테스트를 실행하자.

![image](https://user-images.githubusercontent.com/87057868/135206993-2ca7e692-c55b-4fb9-9006-c13185400caa.png)

빌드가 성공하였다.

새너티 체크 다음으로는 애플리케이션의 주요 기능을 테스트해보자. 여기에서는 HTTP 요청을 처리하고 컨트롤러에 전달하는 MVC 레이어만 테스트할 것이다. 이를 위해 스프링의 MockMvc를 사용할 것인데, 테스트 케이스에 @WebMvcTest 애너테이션을 붙여 주입할 수 있다. 다음 예제로 확인하자.

```java
package com.example.testingrestdocs;

import static org.hamcrest.Matchers.containsString;
import static org.springframework.restdocs.mockmvc.RestDocumentationRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

// JUnit5에서는 RunWith 대신 ExtendWith(SpringExtension.class)를 사용한다.
@RunWith(SpringRunner.class)
@WebMvcTest(HomeController.class)
public class WebLayerTest {

  @Autowired
  private MockMvc mockMvc;

  @Test
  public void shouldReturnDefaultMessage() throws Exception {
    this.mockMvc.perform(get("/"))
        .andExpect(status().isOk())
        .andExpect(content().string(containsString("Hello, World")));
  }
}
```

테스트를 실행하기 전, 맥(MAC) 기준 `command + 쉼표(,)` 버튼을 눌러 Preferences 를 열어 [ Build, Execution, Deployment ] - [ Gradle ] 탭의 [ Run tests using ] (이미지 우측 하단) 선택값을 IntelliJ IDEA 로 변경하자.

![image](https://user-images.githubusercontent.com/87057868/135210699-29d2edfa-2530-4359-823e-4eb1b33b0640.png)

설정을 마친 다음 실행하면 테스트는 통과한다.

![image](https://user-images.githubusercontent.com/87057868/135211727-f789e6f5-b5a5-42ba-822c-4c2544f0c8ea.png)

## 7. 문서 조각(snippet) 생성

이전 테스트에서는 모의 HTTP 요청을 만들고 응답을 확인하였다. 하지만 실제 HTTP API는 동적 콘텐츠를 가지는 경우가 많으므로, 테스트나 문서가 유연하게 사용되면 좋을 것이다. Spring REST Docs는 "스니펫"을 생성하여 그렇게 할 수 있다. 테스트에 애너테이션과 추가적인 어설션(assertion)을 더하여 작업을 수행해보자.

```java
package com.example.testingrestdocs;

import org.junit.jupiter.api.Test;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.restdocs.AutoConfigureRestDocs;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;

import static org.hamcrest.Matchers.containsString;
import static org.springframework.restdocs.mockmvc.MockMvcRestDocumentation.document;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(HomeController.class)
@AutoConfigureRestDocs
public class WebLayerTest {

  @Autowired
  private MockMvc mockMvc;

  @Test
  public void shouldReturnDefaultMessage() throws Exception {
    this.mockMvc.perform(get("/"))
        .andDo(print())
        .andExpect(status().isOk())
        .andExpect(content().string(containsString("Hello, World")))
        .andDo(document("home"));
  }
}
```

새로 추가된 `@AutoConfigureRestDocs` 애너테이션은 **스니펫의 디렉토리 위치**를 인수로 갖는다. 예를 들어, `@AutoConfigureRestDocs(outputDir = "target/snippets")`와 같이 사용할 수 있다. 여기에서는 인수를 전달하지 않으므로 디폴트 경로인 `build/generated-snippets` 가 사용된다.  

`MockMvcRestDocumentation.document` 어설션은 **스니펫 간 식별자 역할**을 하는 문자열을 인수로 취한다. 여기에서는 `home`을 인수로 취했기 때문에,  `build/generated-snippets` 아래 `home`의 스니펫들이 생성된다.



이제 테스트를 실행해보자!

`build` 디렉터리 아래 `generated-snippets` 가 생성되었다!(실행 전에는 없었다.) 
뿐만 아니라 `home` 아래 여러 스니펫이 생성되었다.

![image](https://user-images.githubusercontent.com/87057868/135227911-d3fed6fe-836b-4481-8163-aac358b7ac25.png)

이렇게 REST Docs은 테스트 실행 후 자동으로 **HTTP request 와 response에 대한 기본적인 스니펫들**(Asciidoctor 형식)을 만들어준다. 그리고 **curl과 httpie를 사용하는 명령줄 예제 스니펫**들도 함께 만들어준다.

`document()` 어설션에 인수를 추가한다면, 아래와 같이 새로운 스니펫을 만들 수도 있다.

```java
// src/test/java/com/example/testingrestdocs/WebLayerTest.java

this.mockMvc.perform(get("/"))
  ...
  .andDo(document("home", responseFields(
    fieldWithPath("message").description("The welcome message for the user.")
  ));
```

테스트를 실행하면 `home` 디렉터리 안에 `response-fields.adoc` 이라는 스니펫 파일이 추가된다.

![image](https://user-images.githubusercontent.com/87057868/135216563-8f676d83-e90c-4c64-b3ef-362ddae1620c.png)

아래는 새로 추가된 `response-fields.adoc` 파일에 작성된 내용이다.

![image](https://user-images.githubusercontent.com/87057868/135216640-07ea7d6a-3aeb-4ff1-a8ed-2fdcc29ddff3.png)

만약 필드를 생략하거나 이름이 잘못되면 테스트 실행이 실패하니 주의하자.

> 커스텀 스니펫을 만드는 방법에 대해 알고 싶다면  [Spring REST Docs 공식 문서](https://docs.spring.io/spring-restdocs/docs/current/reference/html5/)를 참조하자.

## 8. 스니펫 사용하기

**스니펫을 사용하려면 프로젝트에 Asciidoctor 콘텐츠가 있어야 하고, 빌드 시 해당 스니펫이 포함되어야 한다.** 
작업을 위해 `src/doc/asciidoc/index.adoc`을 생성하고 아래 내용을 작성하자.

```.adoc
= Getting Started With Spring REST Docs

This is an example output for a service running at http://localhost:8080:

.request
include::{snippets}/home/http-request.adoc[] // http-request 스니펫 포함

.response
include::{snippets}/home/http-response.adoc[] // http-response 스니펫 포함

As you can see the format is very simple, and in fact you always get the same message.
```

맨 위에 있는 `=` 은 해당 줄 텍스트를 **레벨 1 섹션 제목**으로 보여준다.
`request` 및 `response` 앞의 `.` 은 해당 줄의 텍스트를 **캡션**으로 바꿔준다.

두 개의 `include` 지시문은 두 개의 스니펫을 포함하고 있다.  포함된 스니펫의 경로는 `{snippets}` 으로 설정하였다. `{snippets}`를 따로 설정하지 않는다면 디폴트 경로(`build/genereted-snippets`)가 참조된다.

> `{snippets}`를 따로 설정한다면 같은 파일 안에  :snippets: {파일 경로} 이렇게 파일 경로를 지정할 수 있다.
> 예를 들어 다음과 같이 할 수 있다.
>
> = Getting Started With Spring REST Docs
> :snippets: ../../../build/generated-snippets

디폴트 경로를 그대로 이용하므로, 파일에 포함된 스니펫들은 우측 이미지에서처럼 잘 나타나고 있다.

![image](https://user-images.githubusercontent.com/87057868/135233295-7aff0670-fd8d-4a15-8aea-814274cc559b.png)

만약, 잘못된 경로가 설정되었다면, 아래 이미지와 같이 `Unresolved directive ~` 메시지가 나올 것이다.

![image](https://user-images.githubusercontent.com/87057868/135236905-3a2a8d13-c1a5-4988-bbd5-0c61a6effa34.png)

마지막으로 생성한 문서를 브라우저에서 직접 확인하자. 파일 아무 곳에서나 오른쪽 클릭한 다음, 아래 경로를 따라 이동하자.

![image](https://user-images.githubusercontent.com/87057868/135237821-66794a6d-dc19-40ec-8722-b75555dde6fc.png)

원하는 브라우저까지 클릭하고 나면 아래 페이지가 나올 것이다. (다만 엔드포인트는 따로 수정해주어야 한다).

![image](https://user-images.githubusercontent.com/87057868/135238099-767e8e97-a730-493c-827a-c0c579018b26.png)



# 참고 자료

- [Creating API Documentation with Restdocs](https://spring.io/guides/gs/testing-restdocs/)


    

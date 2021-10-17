# @RefreshScope 어노테이션

## 1. 어노테이션 정의
```java
@Target(value={TYPE,METHOD})
@Retention(value=RUNTIME)
@Scope(value="refresh")
@Documented
public @interface RefreshScope
```

## 2. 한줄 설명

@RefreshScope 어노테이션은 **구성(configuration) 변경** 이 있을 때, 이러한 변경을 **빈(또는 컴포넌트)에 런타임에 적용** 하기 위해 사용하는 어노테이션입니다.

## 3. 자세히

- @RefreshScope을 사용하는 빈은 (refresh 이벤트가 있은 후) 런타임 때 리프레시되고, 컴포넌트는 (역시 이벤트 있은 후) 다음 번 메서드 호출시 새로운 인스턴스가 생성됩니다. 새로 생성될 때 인스턴스는 완전히 초기화되며 모든 종속성이 주입됩니다. 

- 빈(bean) 중에는 (초기화시) 한 번만 구성을 주입하여 사용하는 빈이 있습니다. 이 빈의 구성을 새롭게 변경하고자 할 때 @RefreshScope 를 사용합니다. 예를 들어 하나의 데이터소스(DataSource)가 여러 개의 커넥션을 가지고 있습니다. 그리고 이 데이터소스의 데이터베이스 URL을 변경했다고 가정합니다. 이 경우 이미 열려 있는 커넥션을 사용하는 클라이언트는 구성 변경에 상관없이 지금 하고 있는 일을 잘 마무리할 수 있어야 합니다. 그런 다음 해당 풀(pool)의 다음 커넥션을 얻을 때 새로운 URL을 갖는 커넥션을 제공받아야 합니다. 이와 같은 동작을 원활히 수행하기 위해 @RefreshScope을 사용합니다.
  > 빈이 "불변" 이라면 빈에 @RefreshScope 주석을 달거나, 아래 속성 키의 값으로 클래스 이름을 지정해야 합니다. <br>
  > 속성 키: `spring.cloud.refresh.extra-refreshable`

- 다음 메서드 호출시 빈을 강제로 초기화하려면(즉, 기존 빈을 폐기 후 새로운 빈을 생성하려면) 캐시 엔트리(entry)를 무효화해야 합니다. 여기서 캐시는 RefreshScope의 스코프를 말합니다. 스코프(scope, 범위)에는 빈의 초기화 값들이 저장됩니다).
  > 클래스 타입 RefreshScope는 스프링 컨텍스트에 속하는 빈입니다. 이 클래스의refreshAll() 메서드를 통해 타겟이 되는 캐시를 지우고 범위(scope) 안에 있는 모든 빈을 리프레시 할 수 있습니다. refresh(String) 메서드를 사용한다면 개별적으로 빈을 리프레시 할 수 있습니다. HTTP 또는 JMX를 통해 /refresh 엔드포인트에 접속하여 클라이언트(리프레시가 필요한 곳)에 refresh 이벤트를 보낼 수 있습니다. (다음 목차 참고)

- @RefreshScope는 기술적으로는 @Configuration 클래스 안에서 동작하는 어노테이션입니다. 하지만 그렇다고 해서 구성 클래스 안에 있는 모든 @Beans가 전부 @RefreshScope 안에 있음을 의미하진 않습니다. 따라서 @RefreshScope가 달리지 않은 이상 앞으로의 주입 관계에서 달라지는 건 없습니다. @RefreshScope가 있어야지만 업데이트된(다시 빌드되고 종속성이 다시 주입된) @Configuration로부터 빈이 다시 초기화됩니다. 

## 4. refresh 이벤트 생성

- 스프링 2.0 이상에서 /refresh 엔드포인트는 web 요청을 허용하지 않습니다(기본값). 다만 엔드포인트의 활성화는 되어있으니 노출 설정만 바꿔 엔드포인트를 따로 노출해주어야 합니다.
- /refresh 엔드포인트를 노출하려면 애플리케이션에 다음 구성을 추가해야 합니다.
  ```
  management:
    endpoints:
      web:
        exposure:
          include: refresh
  ```

- 이제 리프레시 이벤트를 생성하기 위해 리프레시가 필요한 서비스 URL에  HTTP POST 메서드를 호출합니다.

- **Spring boot 2.0 이상**
  - `http://service_url/actuator/refresh`
  - 참고로, refresh 를 적용하려면 먼저 Spring Actuator가 추가되어 있어야 합니다.
  - 만약 없다면 아래 의존성을 빌드 명세에 추가합니다.
   > implementation ‘org.springframework.boot:spring-boot-starter-actuator’

- **Spring boot 2.0 이상**
  - `http://service_url/refresh`
  - 여기서 service_url을 자신의 프로젝트에 맞는 url 로 변경합니다(개발용이라면 localhost).

## 5. 실습 예제

- [여기 링크](https://thecodinglog.github.io/cloud/spring/java/2019/04/12/cloud-native-java-6.html)의 글을 읽어보면 @RefreshScope을 보다 쉽게 이해할 수 있습니다. 
 
## 6. 참고 자료
- [RefreshScope - javadoc](https://www.javadoc.io/doc/org.springframework.cloud/spring-cloud-commons-parent/1.1.4.RELEASE/org/springframework/cloud/context/config/annotation/RefreshScope.html)
- [Spring Cloud](https://cloud.spring.io/spring-cloud-static/spring-cloud.html#_refresh_scope)
- [spring.getdocs.org](https://spring.getdocs.org/en-US/spring-cloud-docs/spring-cloud-commons/cloud-native-applications/spring-cloud-context:-application-context-services/refresh-scope.html)
 


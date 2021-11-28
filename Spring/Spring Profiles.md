# 스프링 Profiles

## 1. 개요

* 프로필(Profile)은 스프링 프레임워크의 핵심 기능으로, 사용자가 정의한 빈을 서로 다른 프로필(예를 들어, dev, test, prod)에 매핑할 수 있도록 한다.
* 그런 다음 서로 다른 환경에서 서로 다른 프로필을 활성화하여 필요한 빈만 부트스트랩할 수 있도록 지원한다.



## 2. 빈에서 @Profile 사용

* 간단하게 특정 프로필에 속하는 빈을 만드는 방법에 대해 알아보자. @Profile 주석을 사용하여 특정 프로필에 빈을 매핑할 것이다. 어노테이션은 단순히 하나(또는 여러) 프로필의 이름을 사용할 수 있다.
* 개발 중에는 활성화되어야 하지만 프로덕션에는 배포되지 않아야 하는 빈이 있다고 하자.
* 해당 Bean에 dev 프로필 주석을 달면 개발 환경에서만 컨테이너에 표시되게 할 수 있다. 즉, dev 프로필은 프로덕션에서 활성화되지 않습는다.
  ```java
    @Component
    @Profile("dev")
    public class DevDatasourceConfig
  ```
* 참고로 프로필 이름 앞에 NOT 연산자를 추가할 수도 있다(예: !dev).
  ```java
    @Component
    @Profile("!dev")
    public class DevDatasourceConfig
  ```
  


## 3.  XML로 프로필 선언

* 프로필은 XML로 구성할 수도 있다. `<beans>` 태그 안에 해당하는 프로필 속성을 포함하면 된다.
  ```xml
  <beans profile="dev">
    <bean id="devDatasourceConfig" 
      class="org.baeldung.profiles.DevDatasourceConfig" />
  </beans>
  ```
  


## 4. 프로필 설정

* 다음 단계는 해당 Bean이 컨테이너에 등록되도록 프로필을 활성화하고 설정하는 것이다. 이를 수행하는 방법은 여러 가지다.

### 4.1. WebApplicationInitializer 인터페이스를 사용하는 프로그래밍 방식
  
* 먼저 웹 애플리케이션에서 WebApplicationInitializer를 사용하여 프로그래밍 방식으로 ServletContext를 구성할 수 있다.
* WebApplicationInitializer는 활성 프로필(Active Profile)을 설정하기에 매우 편리한 위치다. 
  ```java
    @Configuration
    public class MyWebApplicationInitializer
    implements WebApplicationInitializer {
    
        @Override
        public void onStartup(ServletContext servletContext) throws ServletException {
     
            servletContext.setInitParameter(
              "spring.profiles.active", "dev");
        }
    }
  ```
  
### 4.2. ConfigurableEnvironment를 사용하는 프로그래밍 방식

* 다음으로 환경(environment)에서 직접 프로필을 설정할 수도 있다.
  ```java
    @Autowired
    private ConfigurableEnvironment env;
    ...
    env.setActiveProfiles("someProfile");
  ```


### 4.3. web.xml의 컨텍스트 매개변수

* 마찬가지로 컨텍스트 매개변수를 사용하여 웹 애플리케이션의 web.xml 파일에서 활성 프로필을 정의할 수 있다.
  ```java
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/app-config.xml</param-value>
    </context-param>
    <context-param>
        <param-name>spring.profiles.active</param-name>
        <param-value>dev</param-value>
    </context-param>
  ```


### 4.4. JVM 시스템 매개변수

* 프로필 이름은 JVM 시스템 매개변수를 통해 전달할 수도 있다. 다음 프로필은 애플리케이션 시작 중에 활성화된다.
  ```shell
    -Dspring.profiles.active=dev
  ```
  

### 4.5. 환경 변수

* Unix 환경에서는 환경 변수를 통해 프로필을 활성화할 수도 있다.
  ```shell
    export spring_profiles_active=dev
  ```
  
### 4.6. Maven 프로필

* 스프링 프로필은 spring.profiles.active 구성 속성을 지정하여 Maven 프로필을 통해 활성화할 수도 있다.
* 모든 Maven 프로필에서 spring.profiles.active 속성을 설정할 수 있다.
  ```xml
    <profiles>
        <profile>
            <id>dev</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <spring.profiles.active>dev</spring.profiles.active>
            </properties>
        </profile>
        <profile>
            <id>prod</id>
            <properties>
                <spring.profiles.active>prod</spring.profiles.active>
            </properties>
        </profile>
    </profiles>
  ```
* 해당 값은 아래 application.properties의 `{spring.profiles.active}`를 대체하는 데 사용된다.
  ```properties
    spring.profiles.active={spring.profiles.active}
  ```
* 이제 pom.xml에서 리소스 필터링을 활성화해야 한다.
  ```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
        </resources>
        ...
    </build>
  ```
* 그리고 적용할 Maven 프로필을 전환하기 위해 -P 매개변수를 추가한다.
  ```shell
    mvn clean package -Pprod
  ```
* 이 명령은 prod 프로필용 애플리케이션을 패키징하고, 애플리케이션이 실행 중일 때 spring.profiles.active의 값으로 prod를 적용한다.



### 4.7. 테스트에서의 @ActiveProfile

* @ActiveProfile 주석을 사용하면 활성 상태인 프로필을 매우 쉽게 지정하여 테스트를 수행할 수 있다.
  ```java
    @ActiveProfiles("dev")
  ```
* 지금까지 프로필을 활성화하는 방법을 여러 가지 살펴보았다. 이제 이들 사이의 우선순위를 알아보자.
> 1. Context parameter in web.xml
> 2. WebApplicationInitializer 
> 3. JVM System parameter 
> 4. Environment variable
> 5. Maven profile



## 5. 기본 프로필(Default Profile)

* 프로필을 지정하지 않는 모든 Bean은 기본 프로필에 속한다.
* 스프링은 다른 프로필이 활성화되어 있지 않을 때 spring.profiles.default 속성을 사용하여 기본 프로필을 설정하도록 지원한다.


## 6. 활성 프로필 가져오기(Get Active Profiles)

* 프로그래밍 방식으로 활성 프로필 목록에 접근하는 방식에 대해 알아보자. 방법은 크게 환경(Environment) 또는 spring.active.profile을 사용하는 두 가지 방법이 있다.

### 6.1. 환경 사용

* 다음을 주입하여 Environment 개체에서 활성 프로필에 접근할 수 있다.
  ```java
  public class ProfileManager {
      @Autowired
      private Environment environment;
  
      public void getActiveProfiles() {
          for (String profileName : environment.getActiveProfiles()) {
              System.out.println("Currently active profile - " + profileName);
          }  
      }
  }
  ```

### 6.2. spring.active.profile 사용

* spring.profiles.active 속성을 주입하여 프로필에 접근할 수도 있다.
  ```java
  @Value("${spring.profiles.active}")
  private String activeProfile;
  ```
* 여기에서 activeProfile 변수에는 현재 활성 상태인 프로필의 이름이 포함되며, 프로필이 여러 개인 경우 이름이 쉼표로 구분되어 포함된다.
* 그러나 만약 활성 프로필이 없으면 애플리케이션 컨텍스트가 생성되지 않는다. 변수에 주입할 자리 표시자가 없기 때문에 IllegalArgumentException이 발생한다.
* 이 문제를 피하기 위해 콜론(:) 뒤에 기본값을 정의할 수 있다.
  ```java
  @Value("${spring.profiles.active:}")
  private String activeProfile;
  ```
* 이제 활성 프로필이 없으면 활성 프로필에 빈 문자열만 포함된다.
* 그리고 이전 예와 같이 목록에 접근하려면 activeProfile 변수를 쉼표(,)로 나누어 액세스할 수 있다.
  ```java
  public class ProfileManager {
      @Value("${spring.profiles.active:}")
      private String activeProfiles;
  
      public String getActiveProfiles() {
          for (String profileName : activeProfiles.split(",")) {
              System.out.println("Currently active profile - " + profileName);
          }
      }
  }
  ```



### 7. 예제: 프로필을 사용하여 데이터 소스 구성 분리

* 개발 및 프로덕션 두 가지 환경에서 모두 데이터 소스를 구성해야 한다고 해보자.
* 먼저 두 데이터 소스 구현 모두에서 구현해야 하는 공통 인터페이스 DatasourceConfig를 만들 것이다.
  ```java
  public interface DatasourceConfig {
      public void setup();
  }
  ```
* 다음으로 개발 환경을 위한 데이터 소스를 구성하자.
  ```java
  @Component
  @Profile("dev")
  public class DevDatasourceConfig implements DatasourceConfig {
    @Override
    public void setup() {
      System.out.println("Setting up datasource for DEV environment. ");
    }
  }
  ```
* 그리고 그 다음으로 프로덕션 환경을 위한 데이터 소스를 구성하자.
  ```java
  @Component
  @Profile("production")
  public class ProductionDatasourceConfig implements DatasourceConfig {
    @Override
    public void setup() {
      System.out.println("Setting up datasource for PRODUCTION environment. ");
    }
  }
  ```
* 이제 테스트를 만들고 DatasourceConfig 인터페이스를 주입하자. 활성 프로필에 따라 스프링은 DevDatasourceConfig 또는 ProductionDatasourceConfig 빈을 주입할 것이다.
  ```java
  public class SpringProfilesWithMavenPropertiesIntegrationTest {
      @Autowired
      DatasourceConfig datasourceConfig;
  
      public void setupDatasource() {
          datasourceConfig.setup();
      }
  }
  ```
* 만약 dev 프로필이 활성화되어 있다면 스프링은 DevDatasourceConfig 객체를 주입한다. setup() 메서드를 호출하면 다음과 같은 결과가 출력된다.
  ```shell
  Setting up datasource for DEV environment.
  ```



## 8. 스프링 부트의 프로필

* 스프링 부트는 몇 가지 추가 기능과 함께 지금까지 설명한 모든 프로필 구성을 지원한다.

### 8.1. 프로필 활성화 또는 설정

* 앞의 섹션 4에서 소개한 초기화 매개변수 spring.profiles.active는 스프링 부트에서 속성으로 설정할 수도 있다. 이것은 현재 활성화된 프로필을 정의하기 위해 스프링 부트가 자동으로 선택하는 표준 속성이다.
  ```properties
  spring.profiles.active=dev
  ```
* 하지만 이 속성은 스프링 부트 2.4부터 ConfigDataException(즉, InvalidConfigDataPropertyException 또는 InactiveConfigDataAccessException)이 발생할 수 있으므로 spring.config.activate.on-profile과 함께 사용할 수 없다.
* 프로그래밍 방식으로 프로필을 설정하려면 SpringApplication 클래스를 사용할 수도 있다.
  ```java
  SpringApplication.setAdditionalProfiles("dev");
  ```
* 스프링 부트에서 Maven을 사용하여 프로필을 설정하려면 pom.xml의 spring-boot-maven-plugin에서 프로필 이름을 지정할 수 있다.
  ```java
  <plugins>
    <plugin>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-maven-plugin</artifactId>
          <configuration>
              <profiles>
                  <profile>dev</profile>
              </profiles>
          </configuration>
      </plugin>
      ...
  </plugins>
  ```
* 그리고 스프링 부트 관련 Maven 명령을 실행하자.
  ```shell
  mvn spring-boot:run
  ``` 
  

### 8.2. 프로필별 속성 파일

* 스프링 부트가 제공하는 가장 중요한 프로필 관련 기능은 **프로필별 속성 파일** 이다. 이러한 이름은 **application-{profile}.properties** 형식으로 지정해야 한다.
* **스프링 부트는 모든 프로필에 대해 application.properties 파일의 속성을 자동으로 로드하고 지정된 프로필에 대해서만 프로필별 .properties 파일에 있는 속성을 로드한다.**
* 예를 들어 application-dev.properties 및 application-production.properties라는 두 개의 파일을 사용하여 개발 및 프로덕션 프로필에 대해 서로 다른 데이터 소스를 구성할 수 있다.
* application-production.properties 파일에서는 MySql 데이터 소스를 설정할 수 있다.
  ```properties
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.url=jdbc:mysql://localhost:3306/db
  spring.datasource.username=root
  spring.datasource.password=root
  ```
* 한편, application-dev.properties 파일에서는 동일한 속성에 H2 데이터베이스 데이터 소스를 구성할 수 있다.
  ```properties
  spring.datasource.driver-class-name=org.h2.Driver
  spring.datasource.url=jdbc:h2:mem:db;DB_CLOSE_DELAY=-1
  spring.datasource.username=sa
  spring.datasource.password=sa
  ```
* 이러한 방식으로 다양한 환경에 대해 다양한 구성을 쉽게 제공할 수 있다.
* 스프링 부트 2.4 이전에는 프로필 관련 문서로 프로필을 활성화하는 것이 가능했다. 하지만 이후 버전에서는 이러한 상황에서 프레임워크가 (다시) InvalidConfigDataPropertyException 또는 InactiveConfigDataAccessException을 발생시킨다.


## 8.3. 다중 문서 파일

* 별도의 환경에 대한 속성 정의를 보다 단순히 하기 위해 동일한 파일의 모든 속성을 묶고 구분 기호를 사용하여 프로필을 나타낼 수도 있다.
* 스프링 부트 2.4부터 이전에 지원되는 YAML 외에도 속성 파일에 대한 다중 문서 파일에 대한 지원을 확장했다. 이제 동일한 application.properties에서 dev 및 production 속성을 지정할 수 있다.
  ```properties
  my.prop=used-always-in-all-profiles
  #---
  spring.config.activate.on-profile=dev
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.url=jdbc:mysql://localhost:3306/db
  spring.datasource.username=root
  spring.datasource.password=root
  #---
  spring.config.activate.on-profile=production
  spring.datasource.driver-class-name=org.h2.Driver
  spring.datasource.url=jdbc:h2:mem:db;DB_CLOSE_DELAY=-1
  spring.datasource.username=sa
  spring.datasource.password=sa
  ```
* 이 파일은 스프링 부트에서 위에서 아래로 읽는다. 즉, 위의 예에서 my.prop과 같은 일부 속성이 마지막에 한 번 더 발생하면 가장 마지막 값이 적용된다.


## 8.4. 프로필 그룹

* 스프링 부트 2.4에서는 프로필 그룹 기능이 추가되었다. 이름에서 알 수 있듯이 이제 유사한 프로필을 함께 그룹화할 수 있다.
* 프로덕션 환경에 대해 여러 구성 프로필이 있다고 하자. 그리고 프로덕션 환경의 데이터베이스용 proddb와 스케줄러용 prodquartz가 있다고 해보자.
* application.properties 파일을 통해 이러한 프로필을 한 번에 모두 활성화하려면 다음과 같이 지정할 수 있다.
  ```properties
  spring.profiles.group.production=proddb,prodquartz
  ```
* 결과적으로 프로덕션 프로필을 활성화하면 proddb 과 prodquartz 이 모두 활성화된다.

## [ 참고자료 ]
- [출처](https://www.baeldung.com/spring-profiles)

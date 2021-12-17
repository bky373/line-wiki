# 스프링으로 Hiberate 5 부트스트랩하기

## 1. 개요
* Java 또는 XML 구성을 사용하여 스프링으로 Hibernate 5 를 부트스트랩 하는 방법에 대해 알아본다.


## 2. 스프링과 Hibernate 의 통합
* 네이티브 Hibernate API를 사용하여 SessionFactory 를 부트스트랩하는 것은 다소 복잡하다(복잡하다기보다 귀찮은 코드 추가 작업이 필요하다).
  > [공식 문서](https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#bootstrap-native) 또는 공식 문서의 시작 가이드를 정리한 [이곳](https://github.com/bky373/line-wiki/blob/main/Hibernate/Hibernate%20시작%20가이드.md) 을 참고하자.
* 다행히 스프링은 SessionFactory 를 부트스트랩 을 지원하기 때문에 간단한 자바 코드 몇 줄 또는 XML 구성을 추가하기만 하면 된다.


## 3. Maven 종속성
* 시작하기에 앞서 먼저 `pom.xml` 에 필요한 종속성을 추가해보도록 하자.
    ```xml
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.4.2.Final</version>
        </dependency>
    ```
* 스프링 ORM 모듈은 스프링과 Hibernate 의 통합을 지원한다.
    ```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>5.1.6.RELEASE</version>
        </dependency>
    ```
* 단순하게 작업하기 위해 H2를 데이터베이스로 사용한다.
    ```xml
        <dependency>
            <groupId>com.h2database</groupId> 
            <artifactId>h2</artifactId>
            <version>1.4.197</version>
        </dependency>
    ```
* 마지막으로 Tomcat JDBC Connection Pooling 을 사용하기 위한 종속성을 추가하자. 이는 스프링에서 제공하는 DriverManagerDataSource 보다 프로덕션 목적에 더 적합하다.
    ```xml
        <dependency>
            <groupId>org.apache.tomcat</groupId>
            <artifactId>tomcat-dbcp</artifactId>
            <version>9.0.1</version>
        </dependency>
    ```

## 4. 구성(Configuration)
* 앞에서 얘기했듯이 스프링은 Hibernate SessionFactory 부트스트랩 을 이미 지원한다.
* 따라서 우리는 몇 가지 빈 과 매개변수를 정의하기만 하면 된다.
* 물론 구성을 Java 기반으로 할 것인지, XML 기반으로 할 것인지 까지는 결정해주야 한다.

### 4.1. 자바 구성 사용
* 스프링에서 Hibernate 5 를 사용하기 위해 `org.springframework.orm.hibernate5` 패키지에서 LocalSessionFactoryBean 을 사용해야 한다(`org.springframework.orm.hibernate4` 가 아님에 주의하자).
* 그리고 LocalSessionFactoryBean, DataSource, PlatformTransactionManager 빈 과 일부 Hibernate 특정 속성 을 정의해야 한다(이는 Hibernate 4와 동일하다).
* 스프링으로 Hibernate 5 를 설정하기 위해 HibernateConfig 클래스를 생성하자.
    ```java
        @Configuration
        @EnableTransactionManagement
        public class HibernateConf {
        
            @Bean
            public LocalSessionFactoryBean sessionFactory() {
                LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
                sessionFactory.setDataSource(dataSource());
                sessionFactory.setPackagesToScan(
                  {"com.sample.hibernate.bootstrap.model" });
                sessionFactory.setHibernateProperties(hibernateProperties());
        
                return sessionFactory;
            }
        
            @Bean
            public DataSource dataSource() {
                BasicDataSource dataSource = new BasicDataSource();
                dataSource.setDriverClassName("org.h2.Driver");
                dataSource.setUrl("jdbc:h2:mem:db;DB_CLOSE_DELAY=-1");
                dataSource.setUsername("sa");
                dataSource.setPassword("sa");
        
                return dataSource;
            }
        
            @Bean
            public PlatformTransactionManager hibernateTransactionManager() {
                HibernateTransactionManager transactionManager
                  = new HibernateTransactionManager();
                transactionManager.setSessionFactory(sessionFactory().getObject());
                return transactionManager;
            }
        
            private final Properties hibernateProperties() {
                Properties hibernateProperties = new Properties();
                hibernateProperties.setProperty(
                  "hibernate.hbm2ddl.auto", "create-drop");
                hibernateProperties.setProperty(
                  "hibernate.dialect", "org.hibernate.dialect.H2Dialect");
        
                return hibernateProperties;
            }
        }
    ```

### 4.2. XML 구성 사용
* XML 기반 구성으로 Hibernate 5 를 구성할 수도 있다.
    ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="...">
        
            <bean id="sessionFactory" 
              class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
                <property name="dataSource" 
                  ref="dataSource"/>
                <property name="packagesToScan" 
                  value="com.sample.hibernate.bootstrap.model"/>
                <property name="hibernateProperties">
                    <props>
                        <prop key="hibernate.hbm2ddl.auto">
                            create-drop
                        </prop>
                        <prop key="hibernate.dialect">
                            org.hibernate.dialect.H2Dialect
                        </prop>
                    </props>
                </property>
            </bean>
        
            <bean id="dataSource" 
              class="org.apache.tomcat.dbcp.dbcp2.BasicDataSource">
                <property name="driverClassName" value="org.h2.Driver"/>
                <property name="url" value="jdbc:h2:mem:db;DB_CLOSE_DELAY=-1"/>
                <property name="username" value="sa"/>
                <property name="password" value="sa"/>
            </bean>
        
            <bean id="txManager" 
              class="org.springframework.orm.hibernate5.HibernateTransactionManager">
                <property name="sessionFactory" ref="sessionFactory"/>
            </bean>
        </beans>
    ```
    * XML 구성에서는 먼저 살펴본 Java 기반 구성 에서와 동일한 빈과 매개변수를 정의하고 있다. 
    * 애플리케이션이 Java 구성으로 구성된 경우, XML을 스프링 컨텍스트에 부트스트랩하기 위해 간단한 자바 구성 파일을 사용할 수 있다.
    ```java
        @Configuration
        @EnableTransactionManagement
        @ImportResource({"classpath:hibernate5Configuration.xml"})
        public class HibernateXMLConf {
          //
        }
    ```
  

## 5. 사용하기
* 여기까지 스프링을 통해 Hibernate 5 가 완전히 구성되었다. 따라서 필요할 때마다 raw 한 Hibernate SessionFactory를 직접 주입할 수 있다.
    ```java
       public abstract class FooHibernateDAO {

            @Autowired
            private SessionFactory sessionFactory;
        
            // ...
      }
    ```
  
## 6. 지원되는 데이터베이스
* Hibernate 5 가 지원하는 데이터베이스 Dialect 는 [이곳](https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#database-dialect) 에서 확인할 수 있다.

## 7. 결론
* Java 또는 XML 구성을 사용하여 스프링에서 Hibernate 5 를 구성하는 방법에 대해 살펴보았다. 


## 참고 자료
* [Bootstrapping Hibernate 5 with Spring
  ](https://www.baeldung.com/hibernate-5-spring)

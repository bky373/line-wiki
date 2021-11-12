# 스프링을 사용한 아파치 카프카 소개 

## 1. 개요

- 아래에서는 순수 자바 코드와, 스프링 카프카에서 지원하는 `@KafkaListener` 어노테이션, `KafkaTemplate` 템플릿 모델을 활용하여 카프카 동작(프로듀싱/컨슈밍)을 테스트하고자 한다.

## 2. 설치 및 초기 설정

- 설치는 [공식 가이드](https://kafka.apache.org/quickstart)를 참고한다.

- 카프카 설치 후 IntelliJ에서 새로운 Spring Boot 프로젝트를 하나 생성한다. 설정은 아래 이미지를 참조한다.

  ![image](https://user-images.githubusercontent.com/87057868/139783760-108e9aaa-bdcf-4f1f-af83-a2a48efc4b3d.png)

  ![image](https://user-images.githubusercontent.com/87057868/139783832-bcba83fc-5a4e-489c-91cf-f9132b070def.png)

  - Name을 `kafkasample`로, Type을 `Gradle`로 변경하였고, 나머지는 기본 값을 사용하였다. 그다음 Next를 눌러 이동한다.

  ![image](https://user-images.githubusercontent.com/87057868/139783936-40932fbc-5b92-4f92-977e-6141763b36af.png)

  - 아무 값도 선택하지 않고 Finish를 눌러 프로젝트를 생성한다.

- `kafkasample/build.gradle` 안에 스프링 카프카 의존성을 추가한다.

  ```java
  dependencies {
  	...
    implementation 'org.springframework.kafka:spring-kafka'
    
    ...
    testImplementation 'org.springframework.kafka:spring-kafka-test'
  }
  ```

- 의존성을 추가한 후에는 shift + command + i 키를 눌러 Gradle에 변경사항을 반영한다.

  ![image](https://user-images.githubusercontent.com/87057868/139784158-3346becf-7c3e-428d-b7cb-c25926a4450f.png)

## 3. 토픽(Topic) 구성

- 토픽을 구성하는 방법은 기본적으로 두 가지가 있다.

- **방법 1**: 커맨드라인을 사용한다(하지만 여기에서는 사용하지 않는다).

  ```shell
  ~ kafka-topics --create \
  	--bootstrap-server localhost:9092 
  	--replication-factor 1 
  	--partitions 1 
  	--topic sample-topic
  ```

- **방법 2**: **프로그래밍 방식을 사용한다**(여기에서 사용할 방식이다).

  - 카프카 Admin 클라이언트를 사용한다.
  - 즉, KafkaAdmin 빈(bean)을 추가하여, NewTopic 타입을 가진 모든 빈에 대해 자동으로 토픽을 추가할 수 있게 한다.  

  ```java
  // src/main/java/com/example/kafkasmaple/config/KafkaTopicConfig.java
  // src/main/java/com/example/kafkasample 아래 config 라는 이름의 패키지(폴더)를 따로 만들고,
  // 그 안에 KafkaTopicConfig.java 클래스 파일을 만든다. 
  
  @Configuration
  public class KafkaTopicConfig {
  
    @Value(value = "${spring.kafka.bootstrap-servers}")
    private String bootstrapAddress;
  
    @Bean
    public KafkaAdmin kafkaAdmin() {
      Map<String, Object> configs = new HashMap<>();
      configs.put(AdminClientConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapAddress);
      return new KafkaAdmin(configs);
    }
  
    @Bean
    public NewTopic sampleTopic() {
      return new NewTopic("sampleTopic", 1, (short) 1);
    }
  }
  ```

- `src/main/resources`에 위치한 `application.properties` 설정 파일을 `application.yml`으로 변경한 후, 아래 내용을 작성한다. 이는 위에서 작성한 bootstrapAddress 필드의 @Value 설정 값을 주기 위함이다.

  ```yaml
  spring:
    kafka:
    	bootstrap-servers: localhost:9092
  ```

## 4. 메시지 생성(프로듀싱)

- 메시지를 생성하려면 ProducerFactory를 먼저 구성해야 한다. ProducerFactory에서는 카프카 Producer 인스턴스 생성 전략을 설정한다.
- 그런 다음 KafkaTemplate을 구성한다. KafkaTemplate은 Producer 인스턴스를 감싸며, 사용자가 편리한 방법으로 토픽에 메시지를 보낼 수 있도록 해준다.
- Producer 인스턴스들은 스레드 안전하다. 그래서 애플리케이션 컨텍스트 안에서 단일(single) 인스턴스를 사용하는 게 성능상 유리하다.
- KafkaTemplate 인스턴스도 스레드 안전하기 때문에 하나의 인스턴스를 사용하는 것이 좋다.

### 4-1. 프로듀서(Producer) 구성

```java
// src/main/java/com/example/kafkasmaple/config/KafkaProducerConfig.java

@Configuration
public class KafkaProducerConfig {

  @Value(value = "${spring.kafka.producer.bootstrap-servers}")
  private String bootstrapAddress;

  @Bean
  public ProducerFactory<String, String> producerFactory() {
    Map<String, Object> configProps = new HashMap<>();
    configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapAddress);
    
		// 키(key) 직렬화시 문자열 직렬화 클래스 사용
    configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
    
    // 값(value) 직렬화시 문자열 직렬화 클래스 사용
    configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
    
    return new DefaultKafkaProducerFactory<>(configProps);
  }

  @Bean
  public KafkaTemplate<String, String> kafkaTemplate() {
    return new KafkaTemplate<>(producerFactory());
  }
}
```

- `application.yml`에 producer 설정을 추가하면 다음과 같다.

  ```yaml
  spring:
    kafka:
      bootstrap-servers: localhost:9092
      producer: # 추가
        bootstrap-servers: localhost:9092 # 추가
  ```

### 4-2. 메시지 생성(퍼블리싱) 기능 만들기

- 이제 앞에서 선언한 KafkaTemplate 인스턴스의 send 메서드를 사용하여 특정 토픽에 메시지를 보낼 수 있다.

```java
// src/main/java/com/example/kafkasmaple/service/KafkaProducerService.java
// src/main/java/com/example/kafkasample 아래 service 라는 이름의 패키지(폴더)를 따로 만들고,
// 그 안에 KafkaProducerService.java 클래스 파일을 만들었다. 

@Service
public class KafkaProducerService {

  @Autowired
  private KafkaTemplate<String, String> kafkaTemplate;

  public void sendMessage(String message) {
    kafkaTemplate.send("sampleTopic", message); 
  }
}
```

- 위의 send API는 ListenableFuture 객체를 반환한다. 만약 전송을 하는 스레드(sending thread)를 막고, 이미 보낸 메시지(the sent message)의 결과를 얻으려면 ListenableFuture 객체의 get API를 호출하면 된다. 이때 스레드는 결과를 기다릴 것이고, 프로듀서는 속도가 느려질 것이다.
- 하지만 카프카는 빠르게 메시지를 처리할 수 있는 플랫폼이다. 따라서 다음 메시지가 이전 메시지의 결과를 기다리지 않도록 결과를 비동기적으로 처리하는 것이 좋다.
- 이를 구현하기 위해 callback을 사용할 수 있지만(https://www.baeldung.com/spring-kafka의 4.2 참고), 여기에서는 사용하지 않는다.

## 5. 메시지 소비(컨슈밍)

### 5-1. 컨슈머(Consumer) 구성

- 메시지를 소비하려면 ConsumerFactory와 KafkaListenerContainerFactory를 먼저 구성해야 한다.

  ```java
  // src/main/java/com/example/kafkasample/config/KafkaConsumerConfig.java
  
  @EnableKafka
  @Configuration
  public class KafkaConsumerConfig {
  
    @Value(value = "${spring.kafka.consumer.bootstrap-servers}")
    private String bootstrapAddress;
  
    @Bean
    public ConsumerFactory<String, String> consumerFactory() {
      Map<String, Object> configProps = new HashMap<>();
      configProps.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapAddress);
      
      // 키(key) 직렬화시 문자열 역직렬화 클래스 사용
      configProps.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
      
      // 값(value) 역직렬화시 문자열 역직렬화 클래스 사용
      configProps.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
      
      return new DefaultKafkaConsumerFactory<>(configProps);
    }
  
    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, String> kafkaListenerContainerFactory() {
  
      ConcurrentKafkaListenerContainerFactory<String, String> factory =
          new ConcurrentKafkaListenerContainerFactory<>();
      factory.setConsumerFactory(consumerFactory());
      return factory;
    }
  }
  
  ```

- 이 빈들이 구성되면 @KafkaListener 어노테이션을 사용하여 컨슈머를 구성할 수 있다. 

- 단, 스프링이 관리하는 빈에서 @KafkaListener 어노테이션을 감지할 수 있도록 위 구성(Configuration) 클래스 상단에 @EnableKafka 어노테이션을 달아주어야 한다.

## 5-2. 메시지 소비(컨슈밍) 기능 만들기

```java
// src/main/java/com/example/kafkasample/service/KafkaConsumerService.java

@Service
public class KafkaConsumerService {

  @KafkaListener(topics = "sampleTopic", groupId = "sampleGroupId")
  public void listenSampleGroup(String message) {
    System.out.println("받은 메시지 = " + message);
  }
}
```

## 참고자료

- [Intro to Apache Kafka with Spring](https://www.baeldung.com/spring-kafka)
- [Springboot + Kafka 연동하여 pub/sub 구현 예제](https://oingdaddy.tistory.com/308)


# APACHE KAFKA QUICKSTART
> 아래 내용은 기본적으로 [카프카 공식 가이드 문서](https://kafka.apache.org/quickstart)를 참고하였다. <br>
> 그리고 내용 보완을 위해 맨 아래 참고 자료에 있는 글들을 함께 참고하였다. <br>
> 이 글은 가이드의 STEP 1 에서 STEP 8 까지의 내용만 담고 있다(일부 STEP 생략). <br> 따라서 이미 순서가 명시적으로 드러나 있고, 내용이 그리 길지 않기 때문에 목차를 생략하였다.

> (주의) <br>
> 셸(터미널)에서 명령줄을 실행할 때 가이드에 나온 것과 같이 파일의 위치별로 명령줄을 시작하지 않고, <br>
> 아래 각 STEP 안에 작성한 대로 명령줄을 실행하였다(MAC 기준).

## STEP 1: 카프카 받기

- 생략([가이드](https://kafka.apache.org/quickstart) 참고)

## STEP 2: 카프카 환경 시작하기

- 주키퍼 실행

  ```shell
  # Start the ZooKeeper service
  # Note: Soon, ZooKeeper will no longer be required by Apach Kafka
  ~ zookeeper-server-stop /usr/local/etc/kafka/zookeeper.properties
  ```

- 카프카 브로커 서비스 시작

  ```shell
  # Start the Kafka broker service
  ~ kafka-server-start /usr/local/etc/kafka/server.properties
  
  # 위의 명령을 실행하면, 다음 로그가 생성됨(빨리 올라가서 안 보일 수도..)
  # [2021-10-29 17:20:47,535] INFO Session establishment complete on server localhost/[0:0:0:0:0:0:0:1]:2181 ...
  # [2021-10-29 17:21:00,588] INFO [ZooKeeperClient Kafka server] Connected. (kafka.zookeeper.ZooKeeperClient)
  ```

  > **주키퍼의 역할** ([참고](https://so-easy-coding.tistory.com/19))
  >
  > - 카프카의 메타데이터를 저장
  > - 2.7버전까지는 주키퍼가 필수로 필요함
  > - 상용 운용 환경에서는 반드시 주키퍼를 3대 이상 묶어 구축
  > - 위의 서비스들이 정상적으로 실행되면, 기본적인 카프카 실행 환경을 구축한 것이고, 카프카를 사용할 준비가 된 것이다.

## STEP 3: 이벤트를 저장할 토픽 생성하기

- 토픽 생성

  ```shell
  # Create the Topic
  ~ kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic quickstart-events
  ```

  > **옵션 설명** ([참고](https://kplog.tistory.com/240))
  >
  > - kafka-topics 명령 옵션을 보려면, kafa-topics 만 셸에 입력하면 된다.
  > - **--replication-factor <Integer: replication factor>**: 복제본 개수
  > - **--partitions <Integer: # of partitions>**: 파티션 개수
  > - **--topic <String: topic>**: 토픽 이름 (여기에서는 quickstart-events)
  > - localhost의 포트 9092 는 카프카 서비스 실행시 아래와 같은 로그가 나올 때 
  >   ```shell
  >   [ZooKeeperClient Kafka server] Initializing a new session to localhost:9092. (kafka.zookeeper.ZooKeeperClient)
  >   ```

## STEP 4: 토픽 안에 이벤트 쓰기

- 카프카 클라이언트는 이벤트 쓰기(또는 읽기)를 위해 네트워크를 통해 카프카 브로커와 통신한다.

- 한번 이벤트를 받기 시작하면, 브로커는 필요한 기간 동안(영원히도 가능…) 견고하고 안전하게 이벤트를 저장한다.

- 토픽에 몇 가지 이벤트를 쓰기 위해 (콘솔) 프로듀서 클라이언트를 실행하자. 기본적으로, 새로운 줄마다 새로운 이벤트가 토픽에 쓰인다.

  ```shell
  ~ kafka-console-producer --topic quickstart-events --bootstrap-server localhost:9092
  This is my first event
  This is my second event
  ```

- 이벤트 쓰기를 중지하려면 아무때나 ctrl + c 단축키를 누르면 된다.

## STEP 5: 생성한 이벤트 읽기

- 또다른 셸(터미널)을 열고, (콘솔) 컨슈머 클라이언트 명령을 실행하여 방금 생성한 이벤트를 읽어보자.

  ```shell
  ~ kafka-console-consumer --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
  This is my first event
  THis is my second event
  ```

- 실시간으로 이벤트 읽는 것을 확인하려면, STEP 4를 진행한 셸로 돌아가 새 이벤트를 작성해보자(이미 종료했다면 STEP 4의 콘솔 프로듀서 클라이언트 명령을 다시 실행하자.)

- 나는 콘솔 프로듀서 클라이언트 셸에서 `This is my third event`를 입력하였고, 실시간으로 콘솔 컨슈머 클라이언트 셸에서 해당 메시지가 소비되는 걸 확인하였다.

- 이벤트 읽기를 중지하려면 아무때나 ctrl + c 단축키를 누르면 된다.

## STEP 6: 카프카 커넥트 활용하기(이벤트 데이터를 스트림으로 전환)

- 카프카의 기본적인 동작(즉, 이벤트 읽기/쓰기)만 확인하고자 했기 때문에 이 부분은 생략하였다. 필요한 경우 [공식 가이드](https://kafka.apache.org/quickstart)를 참고하자.

## STEP 7: 카프카 스트림으로 이벤트 처리하기

- STEP 6과 마찬가지의 이유로 생략하였다. 필요한 경우 [공식 가이드](https://kafka.apache.org/quickstart)를 참고하자.

## STEP 8: 카프카 환경 종료하기

- 프로듀서와 컨슈머 클라이언트를 종료하려면 ctrl + c 단축키를 사용하면 된다. 
- 카프카 브로커도 ctrl + c 단축키로 종료할 수 있다.
- 마지막으로, 주키퍼 서버도 ctrl + c 단축키로 종료할 수 있다.

# 참고 자료

- [APACHE KAFKA QUICKSTART](https://kafka.apache.org/quickstart)
- [[Kafka] Apache Kafka 설치 및 실행 (Mac)](https://so-easy-coding.tistory.com/19)
- [아파치 카프카(Kafka) : 설치 및 실행 - 2 of 3](https://kplog.tistory.com/240)


# 카프카 컨슈머 그룹 조회 및 삭제하기

## 1. 개요

* CLI로 카프카 컨슈머 그룹을 조회하거나 삭제할 수 있다.

## 2. 컨슈머 그룹 목록 조회(쿼리)

* 다음 명령을 사용한다.
  ```
  ./kafka-consumer-groups.sh --bootstrap-server {Kafka instance connection address} --list
  
  또는
  
  kafka-consumer-groups --bootstrap-server {Kafka instance connection address} --list
  ```
* 예시
  ```
  [root@zk-server-1 bin]# kafka-consumer-groups --bootstrap-server localhost:9092 --list
  Note: This will not show information about old Zookeeper-based consumers.
  sample-group-1
  test-group-1
  sample-group-2
  test-group-2
  ```
* 위 예시의 컨슈머 그룹 목록
  * `sample-group-1`
  * `test-group-1`
  * `sample-group-2`
  * `test-group-2`

## 3. 컨슈머 그룹 상세 조회

* 특정 컨슈머 그룹을 조회하려면 다음 명령을 사용한다.
  ```
  ./kafka-consumer-groups.sh --bootstrap-server {Kafka instance connection address} --describe --group {consumer group name}

  또는

  kafka-consumer-groups --bootstrap-server {Kafka instance connection address} --describe --group {consumer group name}
  ```
* 예시
  ```
  [root@zk-server-1 bin]#  kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group test-group-2
  Note: This will not show information about old Zookeeper-based consumers.
  Consumer group 'test-group-2' has no active members.

  TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
  test            1          336             336             0               -               -               -
  test            0          334             334             0               -               -               -
  test            2          334             334             0               -               -               -
  ```

## 4. 컨슈머 그룹 삭제

* 특정 컨슈머 그룹을 삭제하려면 다음 명령을 사용한다.
  ```
  ./kafka-consumer-groups.sh --bootstrap-server {Kafka instance connection address} --delete --group {consumer group name}
  또는
  kafka-consumer-groups --bootstrap-server {Kafka instance connection address} --delete --group {consumer group name}
  ```
* 예시
  ```
  [root@zk-server-1 bin]# kafka-consumer-groups.sh --bootstrap-server localhost:9092 --delete --group test-group-2
  Note: This will not show information about old Zookeeper-based consumers.
  Deletion of requested consumer groups ('test-group-2') was successful.
  ```




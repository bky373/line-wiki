# 카프카 토픽 목록 보기

## 1. 개요

* 이번 문서에서는 아파치 카프카 클러스터가 가지고 있는 모든 토픽을 나열하는 방법에 대해 알아본다.
* 먼저 단일 노드(single-node)의 아파치 카프카 와 [Zookeeper](https://www.baeldung.com/java-zookeeper) 클러스터를 설정하는 방법을 살펴볼 것이다. 
* 그런 다음 해당 클러스터가 가지고 있는 토픽들에 대해 살펴볼 것이다.



## 2. 카프카 설정

* 카프카의 토픽을 나열하기 전에 먼저 카프카 설치 및 설정을 완료해야 한다. 
* 단일 노드 카프카 클러스터를 설치 및 설정하기 위해선 아래 3단계 과정을 거쳐야 한다.  
  * 카프카 및 주키퍼 설치
  * 주키퍼 서비스 시작
  * 카프카 서비스 시작
* 위의 3단계를 진행하기 위해 [이곳](https://github.com/bky373/line-wiki/blob/main/Kafka/Kafka%20QuickStart.md) **STEP 2** 까지를 참고하자.     
* 위 문서의 **STEP 2** 까지 정상적으로 진행했다면 클러스터가 제대로 동작하고 있을 것이다. 이제 해당 클러스터에 몇 가지 토픽을 추가해보도록 하자.
  ```shell
    ~ kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic topic-sample1
    ~ kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic topic-sample2
  ```
* 위 명령을 수행했을 때 나타나는 결과 이미지는 아래와 같다.
  <img width="1580" alt="image" src="https://user-images.githubusercontent.com/49539592/145678319-e983a5d4-8549-4c1d-b4c9-d7770623f80f.png">
* 이제 모든 것이 준비되었으니 다음 섹션에서 카프카 토픽을 나열해보자.



## 3. 토픽 나열

* 카프카 설치 번들(bundle)에 포함된 `kafka-topics.sh` 셸 스크림트 또는 `kafka-topics` 명령을 이용하여 클러스터 내의 모든 토픽을 나열할 수 있다.
* 여기에서는 `kafka-topics` 명령을 사용하는데, 명령을 사용할 때 `--list` 옵션과 클러스터 정보를 함께 전달하면 된다. 다음 예시를 보자.
    ```shell
    ~ kafka-topics --list --bootstrap-server localhost:9092
    topic-sample1
    topic-sample2
    ```
* `--list` 옵션은 `kafka-topics.sh` 셸 스크립트에 있는 모든 토픽을 나열하도록 지시한다. 이 경우 위에서 추가한 토픽 2개가 그대로 출력되었다.
* 만약 클러스터에 토픽이 없으면 명령은 특별한 결과를 내지 않고 곧바로 수행을 종료한다.
* `--list` 옵션만 전달해서는 안 된다. 카프카 클러스터와 통신하기 위해 `--bootstrap-server` 옵션으로 Zookeeper 서비스 URL을 함께 전달해야 한다.
* 단일 인스턴스 카프카 클러스터는 9092 포트를 수신하므로 `localhost:9092`를 부트스트랩 서버로 지정하였다.
* 만약 옵션과 URL을 전달하지 않으면 다음과 같은 오류가 발생한다.
  <img width="1299" alt="image" src="https://user-images.githubusercontent.com/49539592/145678781-85f3df6e-da55-41c0-a9b8-a1fd9e68ea96.png">



## 4. 토픽 상세 보기

* 앞에서 토픽 목록을 찾는 방법에 대해 알아봤다면, 이번에는 특정 토픽의 상세 정보를 보는 방법에 대해 알아보자.
* 결론적으로 `--describe -topic <topic name>` 옵션(조합)을 사용하면 특정 토픽의 상세 정보를 알 수 있다. 다음 예시를 보자.
  ```shell
    ~ kafka-topics --describe --bootstrap-server localhost:9092 --describe --topic topic-sample1
  ```
  <img width="1743" alt="image" src="https://user-images.githubusercontent.com/49539592/145679009-57d61139-53ee-4d57-af9f-1dc4ba015e32.png">
* 위에서 알 수 있듯이, 상세 정보에는 파티션과 복제본의 수(the number of partitions and replicas) 등이 나타난다.
* `--list` 때와 마찬가지로 위 명령을 사용할 때는 클러스터와 통신하기 위해 클러스터 정보(boostrap-server 주소)를 전달해주어야 한다.

## 참고 자료
* [출처](https://www.baeldung.com/ops/kafka-list-topics)

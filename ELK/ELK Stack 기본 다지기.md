# ELK Stack 기본 다지기

* **ELK Stack**
	* E, L, K 라는 세 가지 제품을 조합하여 만든 오프소스 제품을 의미한다.
	* ELK 와 Stack 에 대해 조금 더 알아보자.

* **ELK**
	* Elasticsearch, Logstash, Kibana 세 개의 머리글자를 따서 만든 용어다.
	* 여기에 나중에 Beats가 추가되었고, 더 이상 ELK 라는 머리글자를 사용할 수 없기에 
	* 나중에는**Elastic Stack** 이라는 용어가 쓰이기 시작했다.

> Stack 다음으로 E, L, K 와 B 에 대해 각각 알아보기로 하자.

* **Stack**
	* 기본적으로 알고 있는 스택의 의미로 사용된다. (= 더미)
	* 여기에서는 아래 세네 가지 제품의 더미(= 조합)를 의미한다.
	* Stack은 확장 가능성에 열려있는 용어이기도 하다.
	
    <img src=https://user-images.githubusercontent.com/49539592/156795571-98b2d309-f023-4b33-ae9f-993ca7ade5ac.png width=500 >
    
    [이미지 출처](https://www.elastic.co/kr/what-is/elk-stack) 


* **Elasticsearch**
	* 쉽게 
		* 데이터 검색 및 분석을 위한 프로그램이다. (= 검색 엔진)
	* 추가하면
		* 빈도, 인기도, 최근 결과 등 다양한 기준을 설정하여 원하는 데이터를 검색할 수 있다.
		* 정형, 비정형, 위치정보, 메트릭 등 원하는 방법으로 정보를 검색할 수 있다.
		* 데이터를 저장하고, 검색한다는 점에서 RDB와 컨셉이 유사하다 (사용 방식과 용어는 같지 않다).
		* REST API 기반의 웹 인터페이스 위에서 동작하고, JSON 문서를 지원한다.
		* 준실시간(near realtime)으로 동작한다.
			* 문서를 색인화하는 시점부터 검색이 가능해지는 시점까지 약간의 대기 시간이 있다 (대개 1초)
		*  [루신](https://zetawiki.com/wiki/%EB%A3%A8%EC%8B%A0) 을 기반으로 만들어진 검색 서버다.
			* 루신: 자바로 작성된  [풀 텍스트 검색](https://zetawiki.com/wiki/%EC%A0%84%EB%AC%B8%EA%B2%80%EC%83%89) (full text search) 소프트웨어를 말한다.

* **Logstash**
	* 쉽게
		* 로그 및 다양한 데이터들이 통과하는 파이프 통로다. (아래 그림 참고)
		* 참고) stash - 안전하게 숨기는 장소 (git stash 생각)
	* 추가하면
		* Logstash 파이프라인에는 입력 → 필터 → 출력의 세 가지 단계가 있다. (아래 구성 파일 참고)
		* 파이프에서는 인풋의 데이터를 -> 필터를 통해 가공한 후 -> 원하는 곳으로 흘려보낸다.
			* [인풋](https://www.elastic.co/guide/en/logstash/current/input-plugins.html) (input)의 예: 파일, http, 레디스(redis), 카프카, 비츠(beats) 이벤트 등
			* 다양한 플러그인이 지원되기 때문에 형식이나 복잡성과 관계 없이 데이터를 동적으로 수집, 변환, 전송할 수 있다.
		* 예를 들면, 로그 이벤트를 수집하여 적절히 가공한 후 Elasticsearch 로 보낼 수 있다.
			* 이때 가공([필터](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html)) 방식은 다양하게 선택할 수 있다.
			* [출력](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)  단계에서도 꼭 Elasticsearch가 아니라, 카프카, 메일 등 다양한 곳을 선택할 수 있다.
		* 200개 이상의 플러그인 프레임워크를 사용할 수 있기 때문에, 입력, 필터, 출력을 자유롭게 설정할 수 있다. 

      <img src=https://user-images.githubusercontent.com/49539592/156797922-472bd779-20d5-444a-be12-113257536c6a.png width=500 >

        [이미지 출처](https://osc131.tistory.com/106?category=736354) 

      <img src=https://user-images.githubusercontent.com/49539592/156797947-b8b2082f-1024-46c5-88d3-3585117723a0.png width=500 >
      
        [이미치 출처](https://velog.io/@jjongbumeee/Elasticsearch-%EA%B0%9C%EB%85%90) 

* **Kibana**
	* 쉽게
		* (Elasticsearch) 데이터를 시각화하기 위한 도구를 말한다.
	* 추가하면
		* 온갖 데이터를 → 아래 이미지처럼 다양한 그래프(히스토그램, 막대그래프, 파이차트 등)로 표현할 수 있도록 지원한다.

      <img src=https://user-images.githubusercontent.com/49539592/156797972-4681ae7c-25c1-4b41-9baa-82851cf4ffb6.png width=500 >
 
      [이미지 출처](https://www.elastic.co/kr/kibana/) 

* **Beats**
	* 쉽게
		* 단말 장치 데이터 수집기를 의미한다. (아래 그림 참고)
	* 추가하면
		* Logstash와 Elasticsearch로 전송할 데이터를 단말 장치에서 수집한다.
		* 모든 유형의 데이터를 수집하기 위해 다양한 수집기를 사용한다.
			* Filebeat(경량 로그 수집기): 로그 및 파일을 수집, 전달한다.
			* Metricbeat(경량 메트릭 수집기): CPU부터 메모리, Nginx 등 다양한 시스템 서비스 통계 정보를 수집, 전달한다.
			* Packetbeat(경량 네트워크 데이터 수집기): 네트워크 트래픽의 흐름 정보 수집, 전달
			* Auditbeat(경량 감사 데이터 수집기)
			* Winlogbeat(경량 Windows 이벤트 로그 수집기)
			* Heartbeat(가동 시간 모니터링을 위한 경량 데이터 수집기)
			* Functionbeat(클라우드 데이터를 위한 서버리스 수집기)

* **Elastic Stack 요약**
	* ELK + B 의 사용을 요약하면 다음과 같다.
		* B는 파일비트를 예시로 들었다.

      <img src=https://user-images.githubusercontent.com/49539592/156798329-c5a4c305-ef0b-42f5-b0e6-c2204b1fcf06.png width=500 >

        [이미지 출처](https://medium.com/day34/elk-stack%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B7%B8-%EA%B4%80%EC%A0%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0-ca199502beab) 
    
1. 로그를 생성하는 서버들에 Filebeat를 설치하고, 로그를 집계할 서버에 ELK를 설치한다.
2. Filebeat에서 Logstash로 로그를 전송하고, Logstash에서 필터링을 거친 로그들이 Elasticsearch에 저장된다.
3. 저장된 로그는 Kibana를 통해 시각화하여 볼 수 있다.
4. Filebeat 외에도 아래 Beats를 사용할 수 있다.
  
   <img src=https://user-images.githubusercontent.com/49539592/156798477-7b6565fe-4166-4243-b8fa-d93048db938f.png width=500 >
  
    [이미지 출처](https://velog.io/@eunoia/ELK-Stack-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90) 

## 참고 자료
  * [오픈 소스 검색: Elasticsearch, ELK Stack 및 Kibana 개발사 | Elastic](https://www.elastic.co/kr/)  
  * [Logstash Reference [8.0] | Elastic](https://www.elastic.co/guide/en/logstash/current/index.html) 
  * [ELK 스택, Elastic 스택 - 제타위키](https://zetawiki.com/wiki/ELK_%EC%8A%A4%ED%83%9D,_Elastic_%EC%8A%A4%ED%83%9D)  
  * [ELK Stack을 이용한 로그 관제 시스템 만들기](https://medium.com/day34/elk-stack%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B7%B8-%EA%B4%80%EC%A0%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0-ca199502beab)  
  * [ELK Stack 기본 개념](https://velog.io/@eunoia/ELK-Stack-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90)  


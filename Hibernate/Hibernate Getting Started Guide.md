# Hibernate Getting Started Guide

## 머리말

* 사용하고자 하는 데이터의 개체(Objects) 표현 방식과 관계형 데이터베이스(Relational databases)의 표현 방식이 일치하지 않을 경우 개발 시간 및 비용이 많이 소모된다.
* Hibernate 는 위의 문제를 해결할 수 있도록 개발된 **Java 환경에서 사용하는 ORM(Object/Relational Mapping) 솔루션** 이다.
  > 개체/관계형 매핑(Object/Relational Mapping) 이라는 용어는 개체 모델 표현과 관계형 데이터 모델 표현 간의 데이터를 매핑하는 기술을 의미한다.
* Hibernate 는 Java 클래스에서 데이터베이스 테이블로, Java 데이터 유형에서 SQL 데이터 유형으로 매핑을 처리한다.
* 또한 데이터 쿼리 및 검색 기능을 제공하기 때문에 SQL 및 JDBC에서 수동 데이터 처리 시간을 크게 단축시킨다.
* Hibernate의 설계 목표는 개발자의 수동적인 데이터 영속성 관련 프로그래밍 작업을 95% 덜어주는 것에 있다. 

## 1. Hibernate 설치 및 기본 설명
### 1.1. Hibernate 모듈/아티팩트(Modules/Artifacts)
* Hibernate의 기능은 의존성(모듈성)을 분리하기 위해 여러 개의 모듈/아티팩트로 분할된다.  

* **hibernate-core**
    * 메인(코어) Hibernate 모듈이다.
    * ORM 기능들과 여러 API, 다양한 통합 SPI 를 정의한다.

* **hibernate-envers**
    * Hibernate의 과거 엔티티 버전 관리 기능 을 포함한다. 

* **hibernate-spatial**
    * Hibernate의 공간/GIS 데이터 유형 을 지원한다.

* **hibernate-osgi**
    * OSGi 컨테이너에서 실행하기 위한 Hibernate 지원 기능을 포함한다.

* **hibernate-agroal**
    * Agroal 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.

* **hibernate-c3p0**
    * C3P0 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
    
* **hibernate-hikaricp**
    * HikariCP 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
        
* **hibernate-vibur**
    * Vibur DBCP 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
     
* **hibernate-proxool**
    * Proxool 연결 풀링 라이브러리(connection pooling library)를 Hibernate에 통합한다.
     
* **hibernate-jcache**
    * Ehcache 캐싱 스펙(specification)을 Hibernate에 통합하여 호환되는 모든 구현이 second-level 캐시 공급자가 되도록 한다.
     
* **hibernate-ehcache**
    * Ehcache 캐싱 라이브러리를 second-level 캐시 공급자로 Hibernate에 통합한다.


### 1.2. Release Bundle 다운로드
* [이곳](https://sourceforge.net/projects/hibernate/files/hibernate-orm/) 에서 릴리스 번들을 다운로드할 수 있다.
* 각각의 릴리스 번들은 JAR 파일, 문서, 소스 코드 등을 포함하고 있으며, 보다 자세한 구성은 아래와 같다.
    * `lib/required/` 디렉토리는 `hibernate-core` jar 및 이와 관련된 모든 종속성을 포함한다. 이곳의 jar 들은 Hibernate의 어떤 기능이 사용되든 상관없이 클래스 경로(classpath)에서 사용 가능해야 한다.
    * `lib/envers` 디렉토리는 `hibernate-envers` jar 및 이와 관련된 모든 종속성(`lib/required/` 및 `lib/jpa/`에 있는 것 이상)을 포함한다.
    * `lib/spatial/` 디렉토리는 `hibernate-spatial` jar 및 이와 관련된 모든 종속성(`lib/required/`에 있는 것 이상)을 포함한다.
    * `lib/osgi/` 디렉토리는 `hibernate-osgi` jar 및 이와 관련된 모든 종속성(`lib/required/` 및 `lib/jpa/`에 있는 것 이상)을 포함한다.
    * `lib/jpa-metamodel-generator/` 디렉토리는 Criteria API 타입 안전(type-safe) 메타모델을 생성하는 데 필요한 jar를 포함한다.
    * `lib/optional/` 디렉토리는 Hibernate가 제공하는 다양한 연결 풀링(connection pooling)과 second-level 캐시 통합에 필요한 jar 및 종속 항목을 포함한다.


### 1.3. Maven 저장소(Repository) 아티팩트

* Hibernate 아티팩트에 관하여 권한을 가지고 있는 저장소는 JBoss Maven 저장소다. 
* Hibernate 아티팩트는 자동화된 작업을 거쳐 Maven Central에 동기화된다(약간의 지연이 발생할 수 있다).
* Hibernate ORM 아티팩트는 `org.hibernate` groupId 아래 게시된다.




## 참고 자료
* [출처](https://docs.jboss.org/hibernate/orm/5.6/quickstart/html_single/)

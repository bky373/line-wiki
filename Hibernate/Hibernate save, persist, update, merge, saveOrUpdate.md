# Hibernate: save, persist, update, merge, saveOrUpdate

## 1. 개요
* Session 인터페이스에서 지원하는 다음 메소드들 간 차이에 대해 알아본다.
    * save
    * persist
    * update
    * merge
    * saveOrUpdate


## 2. 영속성 컨텍스트의 구현; 세션(Session as a Persistence Context Implementation)
* [Session](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Session.html) 인터페이스는 데이터베이스에 데이터를 저장하는 메서드를 여러 개 지원한다.
    > save, persist, update, merge, saveOrUpdate
* 이 메서드들 간의 차이를 이해하기 위해서 먼저 (영속성 컨텍스트로서 존재하는) 세션의 목적에 대해 이해해야 한다.
* 그리고 세션과 관련 있는 엔티티 인스턴스들의 상태 차이에 대해서도 이해해야 한다.

### 2.1. 엔티티 인스턴스 관리하기
* Hibernate는 객체-관계 매핑을 지원하기도 하지만, **런타임 동안 엔티티를 관리하는 문제** 에도 관심이 있다. 
* 그리고 이러한 문제를 해결하기 위해 **영속성 컨텍스트(Persistence Context)** 라는 개념을 도입하였다.
* 영속성 컨텍스트는 **세션 중에 데이터베이스에 로드하거나 저장한 모든 개체에 대한 컨테이너 또는 first-level 캐시**로 이해할 수 있다.
* 참고로, **세션은 비즈니스 로직에 의해 경계가 정의되는 논리적 트랜잭션** 이다. 
* 영속성 컨텍스트를 통해 데이터베이스 작업을 하고, 모든 엔티티 인스턴스가 이 컨텍스트에 연결되어 있으면, 세션 중 사용하는 데이터베이스 레코드 각각에 대응되는 단일 엔티티 인스턴스가 항상 있어야 한다. 
* Hibernate 에서 영속성 컨텍스트는 `org.hibernate.Session` 인스턴스로 표현된다. JPA의 경우에는, `javax.persistence.EntityManager` 이다.
* Hibernate를 JPA 공급자로 사용하고 EntityManager 인터페이스를 통해 작업할 때 이 인터페이스의 구현은 기본적으로 기저의 Session 객체를 래핑 한다.
* Hibernate Session 의 경우 더 풍부한 인터페이스를 제공하기 때문에 Session 을 직접 사용하는 것이 더 유용할 때도 있다.

### 2.2. 엔티티 인스턴스의 상태
* 애플리케이션의 모든 엔티티 인스턴스는 세션 영속성 컨텍스트 와 관련하여 아래 세 가지 주요 상태 중 하나를 갖는다.
  * **일시적(`transient`)**: 이 인스턴스는 이전에 세션에 연결된 적이 없고, 현재도 연결되어 있지 않다. 또한 데이터베이스에 해당 인스턴스와 대응되는 행이 존재하지 않는다. 일반적으로 데이터베이스에 저장하기 위해 새로 만들어진 개체가 여기에 해당한다.
  * **영속적(`persistent`)**: 이 인스턴스는 고유한 세션 객체와 연결된다. 세션을 데이터베이스로 플러시(flush)할 때 이 엔티티는 데이터베이스에 대응되는 일관된 레코드가 있음이 보장된다.
  * **분리된(`detached`)**: 이 인스턴스는 한 번 (영속적인 상태에서) 세션에 연결된 적이 있지만 지급은 그렇지 않은 경우에 해당한다. 컨텍스트에서 인스턴스를 퇴출하거나(evict), 세션을 지우거나 닫는 경우, 직렬화/역직렬화 프로세스를 통해 인스턴스를 넣는 경우, 인스턴스는 이 상태를 갖는다. 
* 다음의 상태 다이어그램(state diagram)은 세 가지 인스턴스 상태와 이들 상태 간 전환을 일으키는 세션 메서드들을 적절한 위치에 두어 표현하고 있다. 
  <img width="1034" alt="image" src="https://user-images.githubusercontent.com/49539592/146630105-9922ed84-4627-4084-9de3-f12300013ce7.png">
* 엔티티 인스턴스가 영속적(persistent) 상태에 있을 때, 이 인스턴스가 가진 매핑 필드의 모든 변경사항은 세션 플러시 시 해당 데이터베이스 레코드와 필드에 반영된다.
* 영속적(persistent) 인스턴스는 "온라인" 상태로 간주되는 한편, 분리된(detached) 인스턴스는 "오프라인"으로 간주되어 변화를 모니터링하지 않게 된다.
* 이는 영속적(persistent) 객체의 필드 변경 후 이를 데이터베이스에 적용하기 위해 save 나 update 와 같은 메서드를 호출할 필요가 없다는 것을 의미한다.
* 대신 변경 작업이 완료되었을 때 트랜잭션을 커밋하거나 세션을 플러시하거나 닫아주면 된다.



## 참고 자료
* [Hibernate: save, persist, update, merge, saveOrUpdate](https://www.baeldung.com/hibernate-save-persist-update-merge-saveorupdate)


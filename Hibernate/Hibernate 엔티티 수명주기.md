# Hibernate 엔티티 수명주기(Lifecycle)

## 1. 개요
* 모든 Hibernate 엔티티는 프레임워크 내에서 수명주기를 갖는다.
    * 수명주기 상태는 다음과 같을 수 있다.
        * 일시적(transient) 상태
        * 관리되는(managed) 상태
        * 분리된(detached) 상태
        * 삭제된(deleted) 상태
* Hibernate 를 적절하게 사용하기 위해 이들을 반드시 먼저 이해해야 한다.
> 만약 엔티티를 처리하는 다양한 **Hibernate 메서드들** 에 대해 알고 싶다면 [이곳](https://github.com/bky373/line-wiki/blob/main/Hibernate/Hibernate%20Save%2C%20Persist%2C%20Update%2C%20Merge%2C%20SaveOrUpdate.md) 을 참고하자.

## 2. Helper 메서드들
* 이하의 전체 섹션에서 다음 몇몇 Helper 메서드들을 사용할 것이다.
    * `HibernateLifecycleUtil.getManagedEntities(session)`: 세션의 내부 저장소로에서 관리되는 모든 엔티티를 가져오는 데 사용한다.
    * `DirtyDataInspector.getDirtyEntities()`: 'dirty' 로 표시된 모든 엔xl티 목록을 가져오는 데 사용한다.
    * `HibernateLifecycleUtil.queryCount(query)`: 임베디드 데이터베이스에 `count(*)` 쿼리를 수행하는 데 사용한다.
* 위의 모든 Helper 메서드들은 더 나은 가독성을 위해 정적으로 임포트하기로 하자.


## 3. 영속성 컨텍스트에 관한 모든 것
* 엔티티 수명 주기에 대해 알아보기 전에 먼저 영속성 컨텍스트를 이해해야 한다.
* 간단히 말해 **영속성 컨텍스트는 클라이언트 코드와 데이터 저장소 사이에 있다.** 이는 영속적 데이터가 엔티티로 변환되어 클라이언트 코드에서 이를 읽고 변경할 준비가 된 스테이징 영역이다.
* 이론적으로 영속성 컨텍스트는 **[작업 단위(Unit of Work)](https://martinfowler.com/eaaCatalog/unitOfWork.html) 패턴을 구현한 것** 이다.
* **즉, 로드된 모든 데이터를 추적하고 해당 데이터의 변경 사항을 추적하며 결국 비즈니스 트랜잭션이 끝날 때 변경 사항을 데이터베이스에 동기화하는 역할을 수행한다.**
* **JPA EntityManager 및 Hibernate의 Session은 이러한 영속성 컨텍스트 개념을 구현한 것** 이다. 여기에서는 영속성 컨텍스트를 나타내기 위해 Hibernate Session을 사용할 것이다.
* Hibernate 엔티티 수명 주기 상태는 엔티티가 영속성 컨텍스트와 어떻게 연관되어 있는지를 설명한다.


## 4. 관리되는 엔티티(Managed Entity)
* **관리되는 엔티티는 데이터베이스 테이블 행(row)을 표현한 것** 이다(해당 행이 아직 데이터베이스에 있을 필요는 없다).
* 이것은 현재 실행 중인 세션에 의해 관리되며 모든 변경 사항은 추적되어 데이터베이스에 자동으로 전파된다.
* 세션은 데이터베이스에서 엔티티를 로드하거나 분리된 엔티티를 다시 연결한다. 분리된 엔티티는 섹션 5에서 살펴볼 것이다.
* 설명을 위해 다음 샘플 코드를 살펴보자. 샘플 애플리케이션은 `FootballPlayer` 클래스라는 하나의 엔티티를 정의하고, 시작할 때 몇 가지 샘플 데이터로 데이터 저장소를 초기화한다.
  ```shell
    +-------------------+-------+
    | Name              |  ID   |
    +-------------------+-------+
    | Cristiano Ronaldo | 1     |
    | Lionel Messi      | 2     |
    | Gigi Buffon       | 3     |
    +-------------------+-------+
  ```
* 여기에서 Buffon의 이름을 변경하고 싶다고 하자(Gigi Buffon 대신 Gianluigi Buffon 이라는 풀네임을 입력할 것이다).
* 먼저 세션을 획득하여 작업 단위(unit of work)를 시작해야 한다.
  ```java
    Session session = sessionFactory.openSession();
  ```
* 작업 단위의 비즈니스 트랜잭션을 캡슐화하려면 세션이 필요하다. 서버 환경에서는 컨텍스트 인식 프록시(context-aware proxy)를 통해 코드에 세션을 삽입할 수 있다.
* 그런 다음 영속 저장소에서 데이터를 로드하도록 세션에 지시하자.
  ```java
    assertThat(getManagedEntities(session)).isEmpty();

    List<FootballPlayer> players = s.createQuery("from FootballPlayer").getResultList();
    
    assertThat(getManagedEntities(session)).size().isEqualTo(3);
  ```
* 첫 번째 assert 문에서 알 수 있듯이, 세션을 처음 얻었을 때는 영속성 컨텍스트 저장소가 비어 있다.
* 다음으로, 데이터베이스에서 데이터를 검색하고, 데이터의 엔티티 표현을 생성하고, 마지막으로 엔티티를 반환하는 쿼리를 실행한다.
* 내부적으로 세션은, 영속성 컨텍스트 저장소에 로드되는 모든 엔티티를 추적한다. 여기에서는 쿼리 실행 후 세션 내부 저장소에 3개의 엔티티가 포함된다.
* 이제 Gigi의 이름을 변경하자.
  ```java
    Transaction transaction = session.getTransaction();
    transaction.begin();
    
    FootballPlayer gigiBuffon = players.stream()
    .filter(p -> p.getId() == 3)
    .findFirst()
    .get();
    
    gigiBuffon.setName("Gianluigi Buffon");
    transaction.commit();
    
    assertThat(getDirtyEntities()).size().isEqualTo(1);
    assertThat(getDirtyEntities().get(0).getName()).isEqualTo("Gianluigi Buffon");
  ```

### 4.1. 어떻게 작동하는가?
* 트랜잭션 `commit()` 또는 `flush()` 를 호출할 때, 세션은 **추적 목록에서 더티(`dirty`) 엔티티를 찾고 데이터베이스에 해당 엔티티의 상태를 동기화** 한다.
* 세션에 엔티티를 변경했음을 알리기 위해 어떠한 메서드를 호출할 필요가 없었다(이름 값을 바꿀 때 setter 를 사용하고, 기존에 열어놓은 트랜잭션을 종료했을 뿐이다).
* 해당 엔티티가 관리되는(`managed`) 상태였기 떄문에 모든 변경 사항이 데이터베이스에 자동으로 전파될 수 있었다.
* **관리되는 엔티티는 항상 영속적인(`persistent`) 상태의 엔티티이며, 항상 데이터베이스 식별자를 가지고 있어야 한다.**
    * 데이터베이스 행(row) 표현이 아직 생성되지 않은 경우이더라도 식별자를 가지고 있어야 한다(예를 들어, INSERT 문이 아직 작업 단위에 걸려 있는 경우에도).
* 비교를 위해 아래 6. 일시적인 엔티티(Transient Entity) 도 참고하도록 하자.


## 5. 분리된 엔티티(Detached Entity)
* 분리된 엔티티는 **ID 값이 데이터베이스 행(row)의 것과 대응되는 일반적인 엔티티 POJO** 를 뜻한다. 관리되는 엔티티(managed entity)와의 차이점은 **영속성 컨텍스트에서 더 이상 추적하지 않는다** 는 점이다.
* 엔티티를 로드하기 위해 사용된 세션이 닫히거나 `Session.evict(entity)` 또는 `Session.clear()` 를 호출하면 엔티티가 분리된다(`detached`).
* 다음 예시를 보자.
  ```java
    FootballPlayer messi = session.get(FootballPlayer.class, 2L);
    
    assertThat(getManagedEntities(session)).size().isEqualTo(1);
    assertThat(getManagedEntities(session).get(0).getId()).isEqualTo(messi.getId());
    
    session.evict(messi); // detach 상태가 된다
    
    assertThat(getManagedEntities(session)).size().isEqualTo(0);
  ```
    * 영속성 컨텍스트는 이제 분리된 엔티티의 변경 사항을 추적하지 않는다.
  ```java
    messi.setName("MESSI");
    transaction.commit();
    
    assertThat(getDirtyEntities()).isEmpty();
  ```
    * 하지만 `Session.merge(entity)` 또는 `Session.update(entity)` 를 사용하면 세션을 (다시) 연결할 수 있다.
  ```java
    FootballPlayer cr7 = session.get(FootballPlayer.class, 1L);

    session.evict(cr7);
    messi.setName("CR7");
    transaction.commit();
    
    assertThat(getDirtyEntities()).isEmpty();
    
    transaction = startTransaction(session);
    session.update(cr7);
    transaction.commit();
    
    assertThat(getDirtyEntities()).size().isEqualTo(1);
    assertThat(getDirtyEntities().get(0).getName()).isEqualTo("CR7");
  ```
* `Session.merge()` 및 `Session.update()` 을 알고 싶다면 [여기](https://github.com/bky373/line-wiki/blob/main/Hibernate/Hibernate%20Save%2C%20Persist%2C%20Update%2C%20Merge%2C%20SaveOrUpdate.md) 를 참조하자.

### 5.1. 식별자(Identity) 필드가 가장 중요하다
* 다음 로직을 보자.
  ```java
    FootballPlayer gigi = new FootballPlayer();
    gigi.setId(3);
    gigi.setName("Gigi the Legend");
    session.update(gigi);
  ```
    * 위의 예에서 생성자를 통해 일반적인 방식으로 엔티티를 인스턴스화 하였다. 필드를 값으로 채우고 ID를 3으로 설정했다. 이 ID는 Gigi Buffon에 속한 영속 데이터의 ID에 해당한다.
    * 여기서 분리된 엔티티는 단지 ID 값이 영속 데이터의 것과 일치하는 일반 엔티티 를 의미한다.
    * 만약 여기서 `update()` 를 호출하면, 또다른(another) 영속성 컨텍스트에서 이 엔티티를 로드한 것과 같은 효과를 가진다.
      > 사실 세션은 재연결된(re-attached) 엔티티가 어디에서 왔는지 구별하지 않는 특징을 가지고 있다.
    * 여기서 주의할 점이 있다. 만약 업데이트하려는 필드 값만 새로 설정할 경우, 엔티티 전반에 걸쳐 null 값이 생길 수 있고 (업데이트한 필드를 제외한) 나머지 필드는 그 상태 그대로 유지된다는 점이다(따라서 사실상 값이 null로 설정된다).


## 6. 일시적인 엔티티(Transient Entity)
* 일시적인 엔티티는 **영속 저장소에 표시되지도 않고 세션에서도 관리하지 않는 엔티티 객체** 를 말한다.
* **생성자를 통해 새로 생성되는 엔티티의 인스턴스** 가 일시적인 엔티티의 일반적인 예다.
* 일시적인 엔티티를 영속적으로 만들려면 `Session.save(entity)` 또는 `Session.saveOrUpdate(entity)` 를 호출해야 한다. 다음 예시를 보자.
  ```java
    FootballPlayer neymar = new FootballPlayer();
    neymar.setName("Neymar");
    session.save(neymar);
    
    assertThat(getManagedEntities(session)).size().isEqualTo(1);
    assertThat(neymar.getId()).isNotNull();
    
    int count = queryCount("select count(*) from Football_Player where name='Neymar'");
    
    assertThat(count).isEqualTo(0);
    
    transaction.commit();
    count = queryCount("select count(*) from Football_Player where name='Neymar'");
    
    assertThat(count).isEqualTo(1);
  ```
    * **`Session.save(entity)` 를 실행하는 즉시 엔티티에 ID 값이 할당되고 세션에서 관리된다.** 그러나 작업 단위가 끝날 때까지 INSERT 작업이 지연될 수 있기 때문에 데이터베이스에서 아직 사용하지 못할 수도 있다.


## 7. 삭제된 엔티티(Deleted Entity)
* `Session.delete(entity)` 가 호출되고 세션이 엔티티를 삭제 표시한 경우 엔티티는 삭제된(제거된) 상태가 된다. DELETE 명령 자체는 작업 단위(UOW)가 끝날 때 발행될 수 있다.
* 다음 에시를 보자.
  ```java
    session.delete(neymar);

    assertThat(getManagedEntities(session).get(0).getStatus()).isEqualTo(Status.DELETED);
  ```
    * 위에서 알 수 있듯이 작업 단위가 끝날 때까지 엔티티는 영속적 컨텍스트 저장소에 유지된다.


## 참고 자료
* [Hibernate Entity Lifecycle](https://www.baeldung.com/hibernate-entity-lifecycle)

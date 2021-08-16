# Spring in Action

## 책 정보

- 저자 : 크레이그 월즈
- 옮긴이 : 심채철
- 출판사 : 제이펍
- 출간일 : 2020년 05월 14일
- 에디션 : 5th Edition
- 관련링크 :  [실습용 소스 코드](https://github.com/Jpub/SpringInAction5)

## Chapter 분류

- [Chapter 01. 스프링 시작하기 (p3~33)](https://github.com/bky373/line-wiki/blob/main/Spring_In_Action/CH_01_%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0.md#Chapter-01-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)

- [Chapter 02. 웹 애플리케이션 개발하기 (p34~71)](https://github.com/bky373/line-wiki/blob/main/Spring_In_Action/CH_02_%EC%9B%B9%20%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%20%EA%B0%9C%EB%B0%9C%ED%95%98%EA%B8%B0.md#Chapter-02-%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EA%B0%9C%EB%B0%9C%ED%95%98%EA%B8%B0)

## 도메인 객체에 애노테이션 추가하기(p104~)

특정 클래스를 JPA 개체(entity)로 선언하려면 반드시 @Entity 애노테이션을 추가해야 한다. 그리고 이것의 id 속성에는 반드시 @Id 를 지정하여 이 속성이 데이터베이스의 개체를 고유하게 식별한다는 것을 나타내야 한다. 

JPA에서는 개체가 인자 없는 생성자를 가져야 한다. 이를 위해 Lombok의 @NoArgsConstructor를 지정한다. 하지만 인자 없는 생성자의 사용을 원하지 않을 경우, access 속성을 AccessLevel.PRIVATE으로 설정하여 클래스 외부에서 사용하지 못하게 만들 수 있다. 그리고 초기화가 필요한 final 속성이 있다면 force 속성을 true로 설정한다. 이에 따라 Lombok이 자동 생성한 생성자에서 그 속성들을 null로 설정할 수 있다. 아래 예시처럼 작성하면 된다.

```java
@NoArgsConstructor(access=AccessLevel.PRIVATE, force=true)
```

## JPA 리퍼지터리 선언하기(p108~)

JDBC 버전의 리퍼지터리에서는 리퍼지터리가 제공하는 메서드를 개발자가 명시적으로 선언한다.

하지만 스프링 데이터에서는 그 대신 CrudRepository 인터페이스를 확장(extends)할 수 있다. 예를 들어 다음 코드에서는 새로운 인터페이스인 IngredientRepository를 보여준다. CrudRepository 인터페이스에는 데이터베이스의 CRUD(Create(생성), Read(읽기), Update(변경), Delete(삭제)) 연산을 위한 많은 메서드가 선언되어 있다. 

```java
package tacos.data;

import org.springframework.data.repository.CrudRepository;

import tacos.Ingredient;

public interface IngredientRepository extends CrudRepository<Ingredient, String> {
    
    // Iterable<Ingredient> findAll();
    // Ingredient findById(String id);
    // Ingredient save(Ingredient ingredient);
}
```

CrudRepository 인터페이스의 첫 번째 매개변수는 리퍼지터리에 저장되는 개체 타입이고, 두 번째 매개변수는 개체 id 속성의 타입이다. 

인터페이스와 이에 정의된 메소드의 구현을 위해 클래스 구현체를 작성해야 한다고 생각할 수 있다. 하지만 그럴 필요가 없다. 애플리케이션이 시작될 때 스프링 데이터 JPA는 각 인터페이스 구현체(클래스 등)를 자동으로 생성해주기 때문이다. 즉 스프링 데이터 JPA를 사용하면 리퍼지터리만 작성하면 된다. 그리고 JDBC 기반의 구현에서 하듯이 리퍼지터리를 필요한 곳에 주입해주면 된다. 	

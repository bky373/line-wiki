# Spring in Action

## 책 정보

- 저자 : 크레이그 월즈
- 옮긴이 : 심채철
- 출판사 : 제이펍
- 출간일 : 2020년 05월 14일
- 에디션 : 5th Edition
- 관련링크 :  [실습용 소스 코드](https://github.com/Jpub/SpringInAction5)

## 목차

- [Chapter 01. 스프링 시작하기 (p3~33)](https://github.com/bky373/line-wiki/blob/main/Spring_In_Action/CH_01_%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0.md#Chapter-01-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)

- [Chapter 02. 웹 애플리케이션 개발하기 (p34~71)](https://github.com/bky373/line-wiki/blob/main/Spring_In_Action/CH_02_%EC%9B%B9%20%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%20%EA%B0%9C%EB%B0%9C%ED%95%98%EA%B8%B0.md#Chapter-02-%EC%9B%B9-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EA%B0%9C%EB%B0%9C%ED%95%98%EA%B8%B0)

- [Chapter 03. 데이터로 작업하기 (p72~115)](https://github.com/bky373/line-wiki/blob/main/Spring_In_Action/CH_03_%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A1%9C%20%EC%9E%91%EC%97%85%ED%95%98%EA%B8%B0.md#Chapter-03-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A1%9C-%EC%9E%91%EC%97%85%ED%95%98%EA%B8%B0)

- [Chapter 04. 스프링 시큐리티 (p116~166)](https://github.com/bky373/line-wiki/blob/main/Spring_In_Action/CH_04_%EC%8A%A4%ED%94%84%EB%A7%81%20%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0.md#Chapter-04-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0) (정리 중)


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

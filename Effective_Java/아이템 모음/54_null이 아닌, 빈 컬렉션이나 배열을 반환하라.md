# 8장. 메서드(p297~342)

> 이번 장에서는 **메서드를 설계할 때 주의할 점**들을 살펴본다. 
>
> - **매개변수**와 **반환값**을 어떻게 처리해야 하는지
> - **메서드 시그니처**는 어떻게 설계해야 하는지
> - **문서화**는 어떻게 해야 하는지를 다룬다. 
> - 이번 장의 내용 중 상당 부분은 메서드뿐 아니라 **생성자**에도 적용된다. 

## 아이템 54. null이 아닌, 빈 컬렉션이나 배열을 반환하라(p323~325)

컬렉션이나 배열 같은 컨테이너(container)가 비었을 때 null을 반환하지 말자. null을 반환하는 메서드를 사용하면 항상 방어 코드를 넣어줘야 한다. 이런 점에서 아래 코드는 권장하지 않는다.

```java
private final List<Cheese> cheesesInStock = ... ;

/**
 * @return 매장 안의 모든 치즈 목록을 반환한다.
 * 단, 재고가 하나도 없다면 null을 반환한다.
 */
public List<Cheese> getCheeses() {
    return cheesesInStock.isEmpty() ? null
        : new ArrayList<>(cheesesInStock);
}
```

사용자는 getCheeses 메서드를 사용할 때마다 아래 null 방어 코드를 작성해야 한다.

```java
List<Cheese> cheeses = shop.getCheeses();
if(cheeses != null && cheeses.contains(Cheese.STILTON))
	System.out.println("좋았어, 바로 그거야.");
```

위의 코드보다는 빈 컬렉션을 반환하는 아래 코드를 사용하자.

```java
public List<Cheese> getCheeses() {
  return new ArrayList<>(cheesesInStock);
}
```

만약 빈 컬렉션 할당이 성능 문제를 일으킬 것 같다면, 불변 컬렉션을 반환하는 방법을 사용할 수 있다.

```java
public List<Cheese> getCheeses() {
  return cheesesInStock.isEmpty() ?
    Collections.emptyLIst()
    : new ArrayList<>(cheesesInStock);
}
```

배열을 반환한다면 다음 방식을 사용하자. toArray 메서드에 건넨 길이 0짜리 배열은 우리가 원하는 반환 타입(이 경우엔 Cheese())을 알려주는 역할을 한다.

```java
public Cheese[] getCheeses() {
  return cheesesInStock.toArray(new Cheese[0]);
}
```

이 경우에도 길이 0의 배열을 미리 선언함으로써 최적화할 수 있다.

```java
private static final Cheese[] EMPTY_CHEESE_ARRAY = new Cheese[0];

public Cheese[] getCheeses() {
  return cheesesInStock.toArray(EMPTY_CHEESE_ARRAY);
}
```

하지만 다음처럼 배열을 미리 할당하는 건 추천하지 않는다. 오히려 성능이 떨어진다는 연구 결과가 있다.

```java
return cheesesInStock.toArray(new Cheese[cheesesInStock.size()]);
```

## 마무리

- **null이 아닌, 빈 배열이나 컬렉션을 반환하자.** null을 반환하는 API는 사용하기 어렵고 오류 처리 코드도 늘어난다.



# 9장. 일반적인 프로그래밍 원칙(p343~384)

> 이번 장에서는 자바 언어의 핵심 요소라고 할 수 있는 다음 내용들을 다룬다.
>
> - 지역변수
> - 제어구조
> - 라이브러리
> - 데이터 타입 
> - 리플렉션과(언어 경계를 넘나드는 기능) 
> - 네이티브 메서드
> - 최적화와 명명 규칙

## 아이템 58. 전통적인 for 문보다는 for-each 문을 사용하라(p347~350)

for-each 문(정식 이름은, '향상된 for 문(enhanced for statement)')은 언제 쓰는 것이 좋을까? 먼저 다음 예를 보자. 반복자를 사용하여 컬렉션을 순회하는 코드다.

```java
for (Iterator<Element> i = c.iterator(); i.hasNext(); ) {
	Element e = i.next();
	... // e로 무언가를 한다.
}
```

그리고 그 다음은 전통적인 for 문으로 배열을 순회하는 코드다.

```java
for (int i = 0; i < a.length; i++) {
	... // a[i]로 무언가를 한다.
}
```

아이템 57에 따르면 for 문을 사용하는 것이 while 문을 쓰는 것보다 낫다(반복 변수들의 범위를 최소화하기 때문이다). 하지만 위의 두 가지 경우처럼 for 문을 사용했다고 해서 가장 좋은 선택을 한 것은 아니다. 반복자와 많은 인덱스 변수는 한눈에 봐도 코드를 어렵게 하고 오류 발생 가능성을 높인다. 

이렇게 하지 않고 코드를 보다 간결히 사용하고자 할 때 for-each 문은 매우 유용하다. 다음 코드를 보자.

```java
for (Element e : elements) {
	... // e로 무언가를 한다.
}
```

for-each 문에서는 반복자나 인덱스 변수를 사용하지 않기 때문에 훨씬 간결하게 코드를 작성할 수 있다. 컬렉션이나 배열에 상관없이, for-each 문을 사용할 때 반복 속도는 그대로다.

반복문을 중첩해서 사용할 때도 for-each 문은 유용하다. 전통적인 for 문을 사용했을 때 흔하게 실수할 수 있는 다음 예를 보자. 주사위를 두 번 굴렸을 때 나올 수 있는 모든 경우의 수를 출력하고자 한다.

```java
enum Face { ONE, TWO, THREE, FOUR, FIVE, SIX }
...
Collection<Face> faces = EnumSet.allOf(Face.class);

for (Iterator<Face> i = faces.iterator(); i.hasNext(); )
	for (Iterator<Face> j = faces.iterator(); j.hasNext(); )
		System.out.println(i.next() + " " + j.next());
```

결과적으로 위의 코드는 "ONE ONE" 부터 "SIX SIX"까지 단 여섯 쌍만 출력하고 끝난다. 하지만 이는 처음 의도한 "주사위를 두 번 굴렸을 때 나올 수 있는 모든 경우의 수, 즉 36개 숫자 조합이 아니다. 아래 for-each 문을 사용하여 위의 문제를 우아하게 해결할 수 있다.

```java
for (Face face1 : faces)
  for (Face face2 : faces)
    System.out.println(face1 + " " + face2);
```

항상 for-each 문을 사용할 수 있는 것은 아니다. 다음의 세 가지 상황에서는 for-each 문을 쓰지 못한다(주로 반복자나 인덱스를 명시적으로 써야 하는 경우에 해당한다).

- **파괴적인 필터링**(destructive filtering) - 컬렉션을 순회하면서 선택된 원소를 제거해야 한다면 반복자의 remove 메서드를 호출해야 한다. 자바 8부터는 Collection의 removeIf 메서드를 사용해 컬렉션을 명시적으로 순회하는 일을 피할 수 있다.
- **변형**(transforming) - 리스트나 배열을 순회하면서 그 원소의 값 일부 혹은 전체를 교체해야 한다면 리스트의 반복자나 배열이 인덱스를 사용해야 한다.
- **병렬 반복**(parallel iteration) - 여러 컬렉션을 병렬로 순회해야 한다면 각각의 반복자와 인덱스 변수를 사용해 엄격하고 명시적으로 제어해야 한다.

위와 비슷한 경우를 제외하고, for-each 문은 Iterable 인터페이스를 구현한 객체라면 무엇이든 순회할 수 있다.

```java
public interface Iterable<E> {
    // 이 객체의 원소들을 순회하는 반복자 를 반환한다.
	Iterator<E> iterator();
}
```

원소들의 묶음을 표현하는 타입에 (보통 Collection 인터페이스를 구현하겠지만 Collection을 구현하지 않더라도) Iterable을 구현해두면 for-each 문으로 반복문을 사용하는 게 무척 편리할 것이다.

## 마무리

- 전통적인 for 문과 비교했을 때 for-each 문은 명료하고, 유연하고, 버그를 예방해준다. 성능 저하도 없다. 가능한 모든 곳에서 for 문이 아닌 for-each 문을 사용하자.


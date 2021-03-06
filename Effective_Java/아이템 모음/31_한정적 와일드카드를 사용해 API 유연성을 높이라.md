# 5장. 제네릭(p153~206)

## 아이템 31. 한정적 와일드카드를 사용해 API 유연성을 높이라(p181~190)

제네릭 사용시 유연성을 높이는 코드를 짜보려 한다. 이번 아이템에서는 아이템 29에서 봤던 Stack 클래스를 사용할 것이다. 다음은 Stack의 기본적인 public API이다.

```java
public class Stack<E> {
    public Stack();
    public void push(E e);
    public E pop();
    public boolean isEmpty();
}
```

여기에서 더 나아가 스택에 Iterable한 원소를 넣을 수 있는 메서드를 추가한다고 해보자.

```java
public void pushAll(Iterable<E> src) {
    for (E e : src)
        push(e);
}
```

컴파일은 문제 없이 되겠지만 아쉬움이 남는다.

예를 들어 Stack의 실제 타입 매개변수가 `Number`라고 해보자(제네릭 타입으로는 `Stack<Number>`이다). 이 스택에 `Iterable<Integer> ` 타입의 인스턴스를 넣는다고 하면 어떨까? pushAll 메서드를 사용하면 다음과 같이 표현할 수 있다.

```java
Stack<Number> numberStack = new Stack<>();
Iterable<Integer> integers = ...;
numberStack.pushAll(integers);
```

먼저 살펴볼 부분은 Stack의 원소 타입은 `Number`라는 점과 스택에 들어갈 `Iterable`의 원소 타입은 `Integer`라는 점이다. `Number`를 보관하는 곳에 `Integer`를 담는 것은 논리적으로 이상할 게 없어보인다. 하지만 실제로는 다음의 오류 메시지가 출력된다.

```java
StackTest.java:7: error: incompatible types: Iterable<Integer>
    cannot be converted to Iterable<Number>
    numberStack.pushAll(integers);
```

이런 상황에서 **한정적 와일드카드 타입**을 사용하면 오류를 막을 수 있다. 와일드카드 타입을 사용하여 다음과 같이 pushAll 메서드를 수정하자.

```java
public void pushAll(Iterable<? extends E> src) {
    for (E e : src)
        push(e);
}
```

위의 경우에 대입해보면, 와일드카드 타입 `Iterable<? extends E>`는 `Iterable<? extends Number>`이다. 새로 넣을 원소의 `Integer` 타입은 `Number`를 확장하므로 더 이상 컴파일 오류가 발생하지 않는다.

이번에는 pushAll과 반대 기능을 수행하는 popAll 메서드를 새로 추가하자.

```java
public void popAll(Collection<E> dst) {
    while (!isEmpty())
        dst.add(pop());
}
```

 이 역시 컴파일은 문제 없이 끝나겠지만 실제로 사용할 때는 완벽하지 않다. `Stack<Number>`의 원소를 꺼낸 후 `Object`용 컬렉션에 넣을 때 문제가 있다. 우선 다음 코드들도 참고하자.

```java
Stack<Number> numberStack = new Stack<>();
Collection<Object> objects = ...;
numberStack.popAll(objects);
```

Stack의 실제 타입 매개변수는 `Number`이다. popAll의 입력 매개변수 타입이 `Collection<E>`이므로 실제 타입은 `Collection<Number>`임을 알 수 있다. 한편, popAll에 전달되는 인수 타입은 `Collection<Number>`과는 무관한  `Collection<Object>` 이다. 따라서 이대로 컴파일을 하면 "`Collection<Object>`는 `Collection<Number>`의 하위 타입이 아닙니다"라는 오류 메시지가 볼 수 있다.

popAll의 입력 매개변수 타입을 바꿔주도록 하자. Collection이 E 타입 뿐 아니라 E의 상위 타입까지 담을 수 있다면, 위의 문제가 해결되고 메서드를 유연하게 사용할 수 있기 때문이다. 즉, 입력 매개변수의 타입을 `Collection<? super E>`으로 변경한다면 문제를 깔끔하게 해결할 수 있다.

```java
public void popAll(Collection<? super E> dst) {
    while (!isEmpty())
        dst.add(pop());
}
```

두 가지 예시를 통해 알 수 있듯이, **유연성을 극대화하려면 결국 원소의 생산자나 소비자용 입력 매개변수에 와일드카드 타입을 사용해야 한다.** 다만 타입을 정확히 지정해야 하는 상황에서는 와일드카드 타입을 쓰지 말아야 한다(입력 매개변수가 생산자와 소비자 역할을 동시에 하는 경우이다).

## PECS 공식

다음은 와일드카드 타입을 언제 써야 하는지 기억하는 데 도움이 되는 공식이다.

> **펙스(PECS): producer-extends, consumer-super**
>
> 매개변수화 타입 T가 생산자라면 `<? extends T>`를 사용하고, 소비자라면 `<? super T>`를 사용하라.
>
> Stack 예에서 pushAll의 src 매개변수는 Stack이 사용할 E 인스턴스를 생산하므로 src의 적절한 타입은 `Iterable<? extends E>`이다. 
>
> 한편, popAll의 dst 매개변수는 Stack으로부터 E 인스턴스를 소비하므로 dst의 적절한 타입은 `Collection<? super E>`이다.

## PECS 공식 적용

공식을 알았으니 이제 적용해볼 차례다. 앞의 아이템에서 사용했던 메서드와 생성자 선언을 살펴보자.

다음은 아이템 28에서 사용한 Chooser 생성자 선언 코드다.

```java
public Chooser(Collection<T> choices)
```

생성자로 전달되는 choices 컬렉션이 T 타입의 값을 **생산(P)**만 하기 때문에, (펙스 공식에 따라) T를 **확장(E)**하는 와일드카드 타입으로 바꾸어 선언해준다.

```JAVA
public Chooser(Collection<? extends T> choices)
```

이로써 이제 `Chooser<Number>` 클래스 생성자에 `List<Integer>`를 전달하는 것이 가능해졌다.

다음으로 아이템 30에서 다룬 union 메서드를 살펴보자. 

```java
public static <E> Set<E> union(Set<E> s1, Set<E> s2) {
    Set<E> result = new HashSet<>(s1);
    result.addAll(s2);
    return result;
}
```

s1과 s2 모두 E의 **생산**자로 사용되므로, 앞에서처럼 타입 매개변수를 **확장**하는 코드로 바꿔준다.

```java
public static <E> Set<E> union(Set<? extends E> s1, Set<? extends E> s2) {
    Set<E> result = new HashSet<>(s1);
    result.addAll(s2);
    return result;
}
```

참고로 **반환 타입에는 한정적 와일드카드 타입을 사용하면 안 된다.**

이제 변경을 통해 다음과 같은 코드를 작성할 수 있게 되었다.

```java
Set<Integer> integers = Set.of(1, 3, 5);
Set<Double> doubles = Set.of(2.0, 4.0, 6.0);
Set<Number> numbers = union(integers, doubles);
```

## 명시적 타입 인수(explicity type argument)

한편, 자바 7은 자바 8에서만큼 강력한 타입 추론 능력을 가지지 못한다. 이때문에 방금 수정한 union 메서드에서 오류를 발생시킨다(Set.of 팩터리는 다른 코드로 변경해주어야 한다).

```java
Union.java:14: error: incompatible types
    Set<Number> numbers = union(integers, doubles);

  required: Set<Number>
  found: set<INF#1>
  where INT#1, INT#2 are intersection types:
    INT#1 extends Number, Comparable<? extends INT#2>
    INT#2 extends Number, Comparable<?>
```

 이렇게 컴파일러가 올바른 타입을 추론하지 못할 경우에는 **명시적 타입 인수**(explicity type argument)를 작성해주면 된다. 다만, 명시적 타입 인수를 사용하면 코드를 지저분해진다. 어쨌든 다음처럼 명시적 타입 인수를 추가하면 자바 7 이하에서도 깨끗이 컴파일된다(**인수**라고 쓰는 이유는 원래는 타입 '매개변수'가 들어가야 할 자리에 '실젯값'이 들어갔기 때문이다).

```java
Set<Numer> numbers = Union.<Number>union(integers, doubles);
```

## public API를 위한 와일드카드 사용

public API를 위해서라면 타입 매개변수가 들어갈 자리에 와일드카드를 쓰는 것이 좋다. 예를 들어 리스트에서 두 원소의 자리를 교환(swap)하는 정적 메서드를 두 가지 방식으로 정의해보자. 첫 번째는 비한정적 타입 매개변수(아이템 30)를 사용했고 두 번째는 비한정적 와일드카드를 사용했다.

```java
public static <E> void swap(List<E> list, int i, int j);
public static void swap(List<?> list, int i, int j); // 이 방식이 더 낫다.
```

public API에서 두 번째 방식이 더 나은 이유는, 어떤 리스트든 이 메서드에 넘기면 명시한 인덱스의 원소들을 교환해줄 것이기 때문이다. 따라서 **메서드 선언에 타입 매개변수가 한 번만 나오면 와일드카드로 대체하자.** 이때 비한정적 타입 매개변수라면 비한정적 와일드카드로 바꾸고, 한정적 타입 매개변수라면 한정적 와일드카드로 바꾸면 된다.

하지만 이대로 끝내면 안 된다. 두 번째 swap 선언 방식이 컴파일타임에 오류 메시지를 띄우기 때문이다.

```java
public static void swap(List<?> list, int i, int j) {
    list.set(i, list.set(j, list.get(i)));
}
```

다음은 꺼낸 원소를 리스트에 넣을 수 없다는 컴파일 오류 메시지이다.

```java
Swap.java:5: error: incompatible types: Object cannot be converted to CAP#1
    	list.set(i, list.set(j, list.get(i)));
  
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
```

오류가 발생한 이유는 리스트의 타입이 `List<?>`인데, `List<?>`에는 null 외에는 어떤 값도 넣을 수 없기 때문이다.

이를 해결하려면 와일드카드 타입의 실제 타입을 알려주는 도우미 메서드를 따로 작성해야 한다(private으로 관리한다). 결국 swap 메서드 안에 swapHelper 메서드 호출이 추가되었다.

```java
public static void swap(List<?> list, int i, int j) {
    swapHelper(list, i, j);
}

// 와일드카드 타입을 실제 타입으로 바꿔주는 private 도우미 메서드
private static <E> void swapHelper(List<E> lst, int i, int j) {
    list.set(i, list.set(j, list.get(i)));
}
```

실제 타입을 알아내려면 도우미 메서드는 제네릭 메서드로 구현되어야 한다. 외부에서 매끄럽게 메서드를 사용할 수 있도록 하다보니 내부 구현이 좀더 복잡해졌지만, 다수의 사용자를 생각하면 이렇게 하는 게 낫다.



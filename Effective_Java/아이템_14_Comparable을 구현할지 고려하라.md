# 3장. 모든 객체의 공통 메서드(p51~94)

## 아이템 14. Comparable을 구현할지 고려하라(p87~94)

Comparable는 오직 compareTo 메서드 하나만을 갖는 인터페이스이다. **compareTo는 (구현에 따라) equals의 동치성 비교를 수행할 수 있으며, 순서를 비교할 수 있고, 제네릭하다.** 

```JAVA
public interface Comparable<T> {
    int compareTo(T t);
}
```

Comparable을 구현했다는 것은 그 클래스의 인스턴스들에는 **자연적인 순서(natural order)**가 있다는 것을 의미한다. Comparable을 구현한 객체들의 배열은 Arrays.sort를 사용해 정렬할 수 있다.

```java
Arrays.sort(arr);
```

이를 통해 컬렉션이 제공하는 **검색, 극단값 계산, 정렬** 등의 기능을 쉽게 이용할 수 있다.

**사실 자바 플랫폼 라이브러리의 모든 값 클래스와 열거 타입은 Comparable 인터페이스를 구현했다.** 알바벳, 숫자, 기간, 연대와 같이 순서가 명확한 값 클래스를 정의한다면 반드시 Comparable 인터페이스를 구현하자.

## compareTo 메서드의 일반 규약

- equals의 규약과 비슷하다.

> 현재 객체와 주어진 객체의 순서를 비교한다. **현재 객체가 주어진 객체보다 작으면 음의 정수를, 같으면 0을, 크면 양의 정수를 반환한다.** 이 객체와 비교할 수 없는 타입의 객체가 주어지면 ClassCastException을 던진다.
>
> 다음 설명에서 sgn(표현식) 표기는 수학에서 말하는 부호 함수(signum function)를 뜻하며, 표현식의 값이 음수, 0, 양수일 때, -1, 0, 1을 반환하도록 정의했다.
>
> - **대칭성**: 두 객체 참조의 순서를 바꿔 비교해도 예상한 결과가 나와야 한다. 
>   즉, Comparable을 구현한 클래스는 모든 x, y에 대해 
>   **sgn(x.compareTo(y)) == -sgn(y.compareTo(x))** 여야 한다.
>   따라서 x.compareTo(y)는 y.compareTo(x)가 예외를 던질 때에 한해 예외를 던져야 한다.
>
> - **추이성**: Comparable을 구현한 클래스는 **추이성**을 보장해야 한다. 
>   즉, **(x.compareTo(y) > 0 && y.compareTo(z) > 0) 이면, x.compareTo(z) > 0** 이다.
>
> - **반사성**: 필수 사항은 아니지만 다음 부분은 꼭 지키는 게 좋다. 
>   **(x.compareTo(y) == 0) == (x.equals(y))** 여야 한다. Comparable을 구현하고 이 권고를 지키지 않는 모든 클래스는 그 사실을 명시해야 한다. 다음과 같이 명시하면 적당하다.
>
>   " 주의: 이 클래스의 순서는 equals 메서드와 일관되지 않다."

> compareTo 규약을 지키지 못하면 비교를 활용하는 클래스와 어울리지 못한다.

## equals 메서드와의 차이점

compareTo는 equals와 달리 비교대상 타입이 다른 것에 신경 쓰지 않아도 된다. 타입이 다른 객체가 주어지면 일반적으로  ClassCastException을 던진다. 타입이 다른 객체를 비교하고자 할 때는 공통 인터페이스를 구현한 클래스끼리 비교하는 게 좋다.

## BigDecimal 예시

BigDecimal("1.0")과 BigDecimal("1.00") 두 개의 인스턴스를 각각 HashSet 인스턴스와 TreeSet 인스턴스에 추가하자. 그 다음 Set 인스턴스들의 size를 출력해보자.

결과적으로 HashSet 인스턴스의 size는 2개, TreeSet 인스턴스의 size는 1개가 출력될 것이다.

```java
class CompareToSample {

    public static void main(String[] args) {
        Set<BigDecimal> hashSet = new HashSet<>();
        hashSet.add(new BigDecimal("1.0"));
        hashSet.add(new BigDecimal("1.00"));

        Set<BigDecimal> treeSet = new TreeSet<>();
        treeSet.add(new BigDecimal("1.0"));
        treeSet.add(new BigDecimal("1.00"));

        System.out.println(hashSet.size()); // 2
        System.out.println(treeSet.size()); // 1
    }
}
```

같은 인스턴스들을 넣어주었는데 size의 차이는 어디서 발생하는 것일까? BigDecimal의 equals 메서드는 "1.0"과 "1.00"을 서로 다른 인스턴스로 본다. 하지만 BigDecimal의 compareTo 메서드는 두 인스턴스를 동일하게 본다. 

HashSet이 BigDecimal의 equals 메서드를 사용하고, TreeSet이 compareTo 메서드를 사용하면서 차이가 발생했지만 결국 근본적으로는 BigDecimal 클래스의 equals와 compareTo 메서드가 일관되지 않아 생긴 문제다.

## 안티 패턴 코드 1:  관계 연산자 < , > 사용

compareTo 메서드에서 관계 연산자 `<` 와 `>` 사용은 오류를 유발할 수 있다. **대신 Type.compare(T t1, T t2)를 사용하는 것이 좋다.**

```java
public int compareTo(int x, int y) {
    return x < y ? 1 : (x == y) ? 0 : -1;
}
```

## 안티 패턴 코드 2:  값의 차 비교

**'값의 차'**를 기준으로 하는 비교 메서드 역시 사용해서는 안 된다.

```java
static Comparator<Object> hashCodeOrder = new Comparator<>() {
    public int compare(Object o1, Object o2) {
        return o1.hashCode() - o2.hashCode();
    }
};
```

이 방식은 정수 오버플로를 일으키거나 IEEE 754 부동소수점 계산 방식에 따른 오류를 낼 수 있다. 대신 하단의 비교자 코드를 사용하자.

## 권장 코드 1:  박싱 클래스의 정적 compare 메서드

다음 코드는 박싱된 기본 타입 클래스의 정적 compare 메서드를 사용한 예시다. 동시에 **클래스에 핵심 필드가 여러 개일 때** 어떤 필드를 먼저 비교하는 게 좋을지 보여주는 예시이기도 하다.  

```java
public int compareTo(PhoneNumber pn) {
    int result = Short.compare(areaCode, pn.areaCode); // 가장 중요한 필드

    if (result == 0) {
        result = Short.compare(prefix, pn.prefix); // 두 번째로 중요한 필드

        if (result == 0) {
            result = Short.compare(lineNum, pn.lineNum); // 세 번째로 중요한 필드
        }
    }

    return result;
}
```

다음은 위에서 본 [ 안티 패턴 코드 2:  값의 차 비교 ] 를 개선한 코드다.

```JAVA
static Comparator<Object> hashCodeOrder = new Comparator<>() {
    public int compare(Object o1, Object o2) {
        return Integer.compare(o1.hashCode(), o2.hashCOde());
    }
};
```

## 권장 코드 2: Comparator 인터페이스가 제공하는 비교자 생성 메서드

자바 8부터 Comparator 인터페이스는 비교자 생성 메서드(comparator construction method)를 사용하여 메서드 연쇄 방식으로 비교자를 생성할 수 있게 되었다. 이 방식은 가독성이 뛰어나나 성능 저하가 있을 수 있다.

```java
private static final Comparator<PhoneNumber> COMPARATOR =
    comparingInt((PhoneNumber pn) -> pn.areaCode)
    .thenComparingInt(pn -> pn.prefix)
    .thenComparingInt(pn -> pn.lineNum);

public int compareTo(PhoneNumber pn) {
    return COMPARATOR.compare(this, pn);
}
```

다음은 위에서 본 [ 안티 패턴 코드 2:  값의 차 비교 ] 를 비교자 생성 메서드로 개선한 코드다.

```java
// comparingInt 메서드는 키 추출 함수(key extractor function)를 인수로 받아, 
// 그 키를 기준으로 순서를 정하는 비교자를 반환한다.
// 키 추출 함수는 객체 참조를 int 타입 키에 매핑 해준다.
static Comparator<Object> hashCodeOrder =
    Comparator.comparingInt(o -> { return o.hashCode(); });
```


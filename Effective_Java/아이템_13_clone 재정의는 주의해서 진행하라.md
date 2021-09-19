# 3장. 모든 객체의 공통 메서드(p51~94)

## 아이템 13. clone 재정의는 주의해서 진행하라(p77~86)

**Cloneable은 복제해도 되는 클래스임을 명시하는 용도의 인터페이스**이다. 이 인터페이스는 메서드 하나 가지고 있지 않지만 Object의 clone의 동작 방식을 결정한다.

**Cloneable을 구현한 클래스의 인스턴스에서 clone을 호출하면 해당 객체의 필드들을 하나하나 복사한 객체를 반환**한다. 그렇지 않은 클래스의 인스턴스에서 호출하면 CloneNotSupportedException을 던진다.

## Object 명세에 적힌 일반 규약

이 객체의 복사본을 생성해 반환한다. '복사'의 정확한 뜻은 그 객체를 구현한 클래스에 따라 다를 수 있다. 일반적인 의도는 다음과 같다. 어떤 객체 x에 대해 다음 식은 참이다.

```java
x.clone() != x
```

또한 다음 식도 참이다.

```java
x.clone().getClass() == x.getClass()
```

하지만 이상의 요구를 반드시 만족해야 하는 것은 아니다.

한편 다음 식도 일반적으로 참이지만, 역시 필수는 아니다.

```java
x.clone().equals(x)
```

관례상, 이 메서드가 반환하는 객체는 super.clone을 호출해 얻어야 한다. 이 클래스와 (Object를 제외한) 모든 상위 클래스가 이 관례를 따른다면 다음 식은 참이다.

```java
x.clone().getClass() == x.getClass()
```

관례상, 반환된 객체와 원본 객체는 독립적이어야 한다. 이를 만족하려면 super.clone으로 얻은 객체의 필드 중 하나 이상을 반환 전에 수정해야 할 수도 있다.

## 문제 없는 clone 사용의 예: 기본 타입 또는 불변 객체 참조 시

**모든 필드가 기본 타입이거나 불변 객체를 참조하는 경우** clone 메서드는 완벽히 우리가 원하는 복제물을 만들어준다. 아이템 10에서의 PhoneNubmer 클래스가 여기 해당한다. PhoneNumber 클래스의 필드는 모두 기본 타입이다.

```java
@Override
public PhoneNumber clone() {
    try {
        return (PhoneNumber) super.clone();
    } catch (CloneNotSupportedException e) {
        throw new AssertionError(); // 일어날 수 없는 일이다.
    }
}
```



## 문제가 되는 clone 사용의 예: 가변 객체 참조 시

**클래스가 가변 객체를 참조하는 경우 clone 메서드 사용은 원하지 않는 결과를 만들 수 있다.** 다음의 Lotto 클래스는 배열 numbers 필드를 가지고 있다. Lotto 클래스의 clone 메서드가 단순히 super.clone() 의 결과를 반환한다면 어떨까? 

```java
import java.util.Arrays;

class Lotto implements Cloneable {
    
    int[] numbers = {1, 3, 7, 9, 11, 20};

    // 재정의한 메서드의 반환 타입은 상위 클래스의 메서드가 반환하는 타입의 하위 타입일 수 있다.
    // 이를 공변 반환 타이핑(covariant return typing)이라 하며 java 1.5부터 지원한다.
    @Override
    protected Lotto clone() throws CloneNotSupportedException {
        return (Lotto) super.clone();
    }

    public static void main(String[] args) throws CloneNotSupportedException {
        Lotto original = new Lotto();
        Lotto cloned = original.clone();

        System.out.println("================================================================");
        System.out.println(">>> 원본 배열 필드와 복제본의 배열 필드는 서로 같은 객체 잠조를 갖는다.");
        System.out.println("================================================================");
        System.out.println("[ original.numbers == cloned.numbers ] -> " + (original.numbers == cloned.numbers));
        System.out.println("[ cloned.numbers.hashCode ] -> " + original.numbers.hashCode());
        System.out.println("[ cloned.numbers.hashCode ] -> " + cloned.numbers.hashCode());

        System.out.println("================================================================");
        System.out.println(">>> 변경 전의 당첨번호");
        System.out.println("================================================================");
        System.out.println("[ original.numbers ] -> " + Arrays.toString(original.numbers));
        System.out.println("[ cloned.numbers ] -> " + Arrays.toString(cloned.numbers));
        System.out.println("================================================================");

        cloned.numbers[0] = 45;
        System.out.println("cloned.numbers[0] = 45;");
        System.out.println(">>> 복제본 번호 변경 후의 당첨번호");
        System.out.println("================================================================");
        System.out.println("[ original.numbers ] -> " + Arrays.toString(original.numbers));
        System.out.println("[ cloned.numbers ] -> " + Arrays.toString(cloned.numbers));
        System.out.println("================================================================");
        System.out.println(">>> 복제본의 번호만 바꿨을 뿐인데 원본의 번호까지 변경되었다.");
        System.out.println("================================================================");
    }
}
```

복제본의 numbers 필드는 Lotto 원본 인스턴스와 똑같은 배열을 참조할 것이다. **원본이나 복제본 중 하나를 수정하면 다른 하나도 수정되어 불변식을 해칠 수 있다**. 이 경우 프로그램이 이상하게 동작하거나 NullPointerException을 던질 수 있다. 위의 코드를 실행하면 다음처럼 당첨번호가 바뀌는 심각한 문제가 발생한다.

```java
================================================================
>>> 원본 배열 필드와 복제본의 배열 필드는 서로 같은 객체 잠조를 갖는다.
================================================================
[ original.numbers == cloned.numbers ] -> true
[ cloned.numbers.hashCode ] -> 460141958
[ cloned.numbers.hashCode ] -> 460141958
================================================================
>>> 변경 전의 당첨번호
================================================================
[ original.numbers ] -> [1, 3, 7, 9, 11, 20]
[ cloned.numbers ] -> [1, 3, 7, 9, 11, 20]
================================================================
cloned.numbers[0] = 45;
>>> 복제본 번호 변경 후의 당첨번호
================================================================
[ original.numbers ] -> [45, 3, 7, 9, 11, 20]
[ cloned.numbers ] -> [45, 3, 7, 9, 11, 20]
================================================================
>>> 복제본의 번호만 바꿨을 뿐인데 원본의 번호까지 변경되었다.
================================================================
```

**clone은 원본 객체에 아무런 해를 끼치지 않는 동시에 복제된 객체의 불변식을 보장해야 한다.** 엄밀하게 따졌을 때 그래야지만 clone 메서드가 생성자와 같은 효과를 가진다. Lotto 클래스의 clone을 제대로 동작하게 하려면 Lotto 내부 정보(numbers)를 따로 복사해야 한다. 배열 객체 numbers를 복사하는 가장 쉬운 방법은 아래와 같이 배열의 기본 clone 메서드를 호출하는 것이다.

```java
@Override
protected Lotto clone() throws CloneNotSupportedException {
    
    Lotto result = (Lotto) super.clone();
    result.numbers = this.numbers.clone(); // numbers 따로 복제
    return result;
}
```

clone 메서드를 위의 코드로 대체했을 때 나타나는 결과는 다음과 같다.

```java
================================================================
>>> 원본 배열 필드와 복제본의 배열 필드는 이제 서로 다른 객체를 참조한다.
================================================================
[ original.numbers == cloned.numbers ] -> false
[ cloned.numbers.hashCode ] -> 460141958
[ cloned.numbers.hashCode ] -> 1163157884
================================================================
>>> 변경 전의 당첨번호
================================================================
[ original.numbers ] -> [1, 3, 7, 9, 11, 20]
[ cloned.numbers ] -> [1, 3, 7, 9, 11, 20]
================================================================
cloned.numbers[0] = 45;
>>> 복제본 번호 변경 후의 당첨번호
================================================================
[ original.numbers ] -> [1, 3, 7, 9, 11, 20]
[ cloned.numbers ] -> [45, 3, 7, 9, 11, 20]
================================================================
>>> 복제본의 번호 변경은 원본 번호에 아무런 영향을 끼치지 못한다.
================================================================
```

배열은 clone 기능을 제대로 구현한 유일한 예이므로, **배열을 복제할 때는 배열의 clone 메서드를 사용하는 것을 가장 권장한다.**

## clone을 완벽히 구현한 배열 clone의 예  

> 배열의 clone이 clone의 완벽한 예시가 되므로 공부할 겸 따로 정리해보았다.

- 배열의 clone 메서드는 **깊은 복사(deepcopy)**를 수행한다.
- 즉, 배열 a를 복제(clone)하면 **물리적으로 다른 객체**가 생성된다(논리적 동치성은 보장). 이는 clone 사용의 **안전한 예**이다.
- 이를 증명하기 위해 아래 Object의 기본 equals와 hashCode를 사용해보자. 배열 a와 배열 b가 서로 다른 객체임을 알 수 있다.
- 하지만 객체 참조가 다르다고 다른 값을 가지는 것은 아니다. Arrays 클래스의 기본 equals와 hashCode를 사용해보면(이들은 논리적 동치성을 확인한다), **배열 a와 배열 b는 (논리적으로) 동일한 객체**로 여겨진다. 

```java
public static void main(String[] args) {
    int[] a = {1,2,3};
    int[] b = a.clone();

    System.out.println(a); // [I@1b6d3586
    System.out.println(b); // [I@4554617c
    System.out.println(a.equals(b)); // false
    System.out.println(b.equals(a)); // false
    System.out.println(a.hashCode()); // 460141958
    System.out.println(b.hashCode()); // 1163157884
    
    System.out.println(Arrays.hashCode(a)); // 30817
    System.out.println(Arrays.hashCode(b)); // 30817
    System.out.println(Arrays.equals(a, b)); // true
}
```

## 복사 생성자와 복사 팩터리

Cloneable을 이미 구현한 클래스를 확장한다면 어쩔 수 없이 clone을 잘 작동하도록 구현해야 한다. 그렇지 않은 상황에서는 **복사 생성자와 복사 팩터리라는 더 나은 객체 복사 방식을 제공할 수 있다.**

**복사 생성자**

```java
public Yum(Yum yum) { ... };
```

**복사 팩터리**

```java
public static Yum newInstance(Yum yum) { ... };
```

복사 생성자와 복사 팩터리 메서드는 Clonable/clone 방식보다 좋은 점이 많다.

- 언어 모순적이고 위험한 객체 생성 메커니즘(super.clone())을 사용하지 않는다.
- clone 규약에 기대지 않는다.
- 정상적인 final 필드 용법과 충돌하지 않는다.
- 불필요한 검사 예외를 던지지 않는다.
- 형변환이 필요없다.
- 인터페이스 타입의 인스턴스를 인수로 받을 수 있다.

## 마지막 정리

위에서 따로 언급하지는 않았지만, Cloneable은 예외 처리, 생성자 내 재정의 메서드 호출 금지, 스레드 안전성 등 주의할 부분이 많이 있다. 따라서 **'복제 기능은 배열을 제외하고 생성자와 팩터리를 이용하는 게 가장 좋다'.**


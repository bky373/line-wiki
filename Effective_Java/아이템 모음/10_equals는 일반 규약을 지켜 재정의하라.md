# 3장. 모든 객체의 공통 메서드(p51~94)

## 아이템 10. equals는 일반 규약을 지켜 재정의하라(p52~66)

> Object에서 final이 아닌 메서드(equals, hashCode, toString, clone, finalize)는 모두 재정의(overriding)를 염두에 두고 설계된 것이라 재정의 시 지켜야 하는 일반 규약이 명확히 정의되어 있다.

## 1. equals를 재정의해야하는 경우 

> 객체 식별성(object identity; 두 객체가 물리적으로 같은가)이 아니라 **논리적 동치성** 을 확인해야 하는데, **상위 클래스의 equals가 논리적 동치성을 비교하도록 재정의되지 않았을 때다.** 주로 값 클래스들이 여기 해당한다. 값 클래스란 Integer와 String처럼 값을 표현하는 클래스를 말한다.

## 2. Object 명세에 적힌 규약

equals 메서드는 **동치관계**(equivalence relation)를 구현하며, 다음을 만족한다.

- **반사성(relfexivity)**: null이 아닌 모든 참조 값 x에 대해, x.equals(x)는 true다.
- **대칭성(symmetry)**: null이 아닌 모든 참조 값 x, y에 대해, x.equals(y)가 true면 y.equals(x)도 true다.
- **추이성(transivitiy)**: null이 아닌 모든 참조 값 x, y, z에 대해, x.equals(y)가 true이고 y.equals(z)도 true이면 x.equals(z)도 true다.
- **일관성(consistency)**: null이 아닌 모든 참조 값 x, y에 대해, x.equals(y)를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환한다.
- **null 아님**: null이 아닌 모든 참조 값 x에 대해, x.equals(null)은 false다.

## 3. 양질의 equals 메서드 구현 방법

1. **== 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다.** 자기 자신이면 true를 반환한다. 성능 최적화용으로 작성하는 코드다.
2. **instanceof 연산자로 입력이 올바른 타입인지 확인한다.** 그렇지 않다면 false를 반환한다. 이때의 올바른 타입은 equals가 정의된 클래스인 것이 보통이지만, 가끔은 그 클래스가 구현한 특정 인터페이스가 될 수도 있다.
3. **입력을 올바른 타입으로 형변환한다.** 앞서 2번에서 instanceof 검사를 했기 때문에 이 단계는 100% 성공한다.
4. **입력 객체와 자기 자신의 대응되는 '핵심' 필드들이 모두 일치하는지 하나씩 검사한다.** 모든 필드가 일치하면 true를, 하나라도 다르면 false를 반환한다. 2단계에서 인터페이스를 사용했다면 입력의 필드 값을 가져올 때도 그 인터페이스의 메서드를 사용해야 한다.

다음은 이상의 비법에 따라 작성해본 PhoneNumber 클래스용 equals 메서드다.

```java
public final class PhoneNumber {
    private final short areaCode, prefix, lineNum;

    public PhoneNumber(short areaCode, short prefix, short lineNum) {
        this.areaCode = rangeCheck(areaCode, 999, "지역코드");
        this.prefix = rangeCheck(prefix, 999, "프리픽스");
        this.lineNum = rangeCheck(lineNum, 9999, "가입자 번호");
    }

    private static short rangeCheck(int val, int max, String arg) {
        if (val < 0 || val > max) {
            throw new IllegalArgumentException(arg + ": " + val);
        }

        return (short) val;
    }

    @Override
    public boolean equals(Object o) {
        if (o == this) {
            return true;
        }

        if (!(o instanceof PhoneNumber)) {
            return false;
        }

        PhoneNumber pn = (PhoneNumber) o;
        return pn.lineNum == lineNum && pn.prefix == prefix
                && pn.areaCode == areaCode;
    }

    ... // 나머지 코드는 생략
}
```



## 4. equals를 재정의하지 않는 경우

- **각 인스턴스가 본질적으로 고유한 경우**. 예를 들면 Thread는 값이 아닌 동작하는 개체로 각 인스턴스를 식별한다.
- **인스턴스의 '논리적 동치성(logical equality)'을 검사할 일이 없는 경우.**
- **상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는 경우.** 대부분의 Set 구현체는 AbstractSet이 구현한 equals를 상속받아 쓰고, List 구현체들은 AbstractList로부터, Map 구현체들은 AbstractMap으로부터 상속받아 그대로 쓴다.
- **클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없는 경우.** 실수로라도 equals 메서드가 호출되는 걸 막으려면 다음과 같이 구현하자.
  ```java
  @Override
  public boolean equals(Object o) {
      throw new AssertionError(); // 호출 금지!
  }
  ```
- **Enum 클래스를 쓰는 경우.** 인스턴스가 둘 이상 만들어지지 않으므로 equals를 재정의하지 않아도 된다. 이런 경우 어차피 논리적 동치성과 객체 식별성이 사실상 똑같은 의미를 갖게 된다.

## 5. 마지막 주의 사항

- **equals를 재정의할 땐 hashCode도 반드시 재정의하자**(아이템 11).
- **너무 복잡하게 해결하려 들지 말자.** 필드들의 동치성만 검사해도 equals 규약을 어렵지 않게 지킬 수 있다.
- **Object 외의 타입을 매개변수로 받는 equals 메서드는 선언하지 말자.** 이는 Object.equals 재정의가 아니라 다중정의다.
  ```java
  // 잘못된 예 - 매개변수 타입은 반드시 Object여야 한다!
  public boolean equals(MyClass o) {
      ...    
  }
  ```


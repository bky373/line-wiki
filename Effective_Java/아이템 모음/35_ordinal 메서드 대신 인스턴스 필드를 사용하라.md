# 6장. 열거 타입과 애너테이션(p207~252)

## 아이템 35. ordinal 메서드 대신 인스턴스 필드를 사용하라(p221~222)

## ordinal 메서드의 한계

모든 열거 타입은 ordinal 메서드를 지원한다. ordinal은 서수 라는 뜻이며, 이 메서드는 하나의 상수가 열거 타입 안에서 몇 번째 상수인지를 정숫값으로 알려준다. 

ordinal 값을 사용하면 상수 안에 순서값을 지정하지 않아도 되므로 왠지 편리할 것 같다. 다음 코드를 보자. 합주단의 종류를 열거 타입 상수로 나타냈으며, 연주자가 1명일 때인 솔로(sole)부터 10명일 때인 디텍트(detect)까지 정의해두었다.

```java
public enum Ensemble {
    SOLO, DUET, TRIO, QUARTET, QUINTET,
    SEXTET, SEPTET, OCTET, NONET, DECET;
    
    public int numberOfMusicians() {
        return ordinal() + 1;
    }
}
```

사용할 수는 있지만 ordinal 정숫값에 의존하는 방식은 많은 단점을 가지고 있다. 

- **상수 선언 순서를 바꿀 순 있지만 의도와 달라질 수 있다.** 심지어 많은 곳에서 사용하는 경우 수정 자체가 불가능하다.
- **이미 사용 중인 정숫값과 같은 값을 가지는 상수를 추가할 수 없다.**
- **중간에 값을 비울 수 없으니 더미(dummy) 상수를 추가해야 한다.** 예를 들어 3중주 4중주(triple quartet, 12명 합주단)를 추가하는 경우, 11명으로 구성된 합주단이 없음에도 이를 더미 상수로 채워줘야만 한다. 쓰이지 않는 값이 많아지면 실용성이 떨어진다.

## ordinal 메서드의 대안: 인스턴스 필드

**ordinal 메서드의 문제를 해결하려면 인스턴스 필드를 사용하면 된다.** 다음은 위 Ensenble 코드에 인스턴스 필드를 추가한 코드다.

```java
public enum Ensemble {
    SOLO(1), DUET(2), TRIO(3),
    QUARTET(4), QUINTET(5), SEXTET(6),
    SEPTET(7), OCTET(8), DOUBLE_QUARTET(8),
    NONET(9), DECET(10), TRIPLE_QUARTET(12);

    private final int numberOfMusicians;

    Ensemble(int numberOfMusicians) {
        this.numberOfMusicians = numberOfMusicians;
    }

    public int numberOfMusicians() {
        return numberOfMusicians;
    }
}
```

> Enum의 API 문서를 보면 ordinal에 대해 이렇게 쓰여 있다. "대부분 프로그래머는 이 메서드를 쓸 일이 없다. 이 메서드는 EnumSet과 EnumMap 같이 열거 타입 기반의 범용 자료구조에 쓸 목적으로 설계되었다." 따라서 이런 용도가 아니라면 ordinal 메서드는 절대 사용하지 말자.


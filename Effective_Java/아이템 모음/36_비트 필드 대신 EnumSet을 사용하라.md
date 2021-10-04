# 6장. 열거 타입과 애너테이션(p207~252)

## 아이템 36. 비트 필드 대신 EnumSet을 사용하라(p223~225)

## 비트 필드 방식

열거한 값들이 각각의 경우로도 쓰이지만 집합으로 사용될 경우, 각 상수에 2의 거듭게곱 값을 할당한 정수 열거 패턴(아이템 34)이 많이 사용되어 왔다. 예를 들어 Text 스타일을 위한 상수를 사용할 때 다음 방식을 사용하였다. 

```java
public class Text {
    public static final int STYLE_BOLD = 1 << 0; // 1
    public static final int STYLE_ITALIC = 1 << 1; // 2
    public static final int STYLE_UNDERLINE = 1 << 2; // 4
    public static final int STYLE_STRIKETHROUGH = 1 << 3; // 8

    // 매개변수 styles는 0개 이상의 STYLE_ 상수를 비트별 OR한 값이다.
    public void applyStyles(int styles) { ... }
}
```

비트별 OR를 사용할 경우 다음처럼, 여러 개의 상수를 하나의 집합으로 모아 사용할 수 있었다.

```java
text.applyStyles(STYLE_BOLD | STYLE_ITALIC);
```

**비트 필드**(bit field)는 이렇게 만들어진 집합을 의미한다. 비트 필드는 비트별 연산을 사용하여 합집합과 교집합 같은 집합 연산을 효율적으로 수행하게 해준다.

하지만 비트 필드는 여러 단점을 가지고 있다.

- 정수 열거 상수의 단점을 그대로 가진다.
- 비트 필드 출력 값은 정수 열거 상수보다 해석하기 훨씬 어렵다.
- API 작성시 최대 사용 비트를 예측하여 미리 타입을 지정해야 한다(int나 long). API를 수정해야만 비트 수(32비트 또는 64비트)를 늘릴 수 있다.

## EnumSet 방식

EnumSet 클래스는 비트 필드의 여러 단점을 해결해준다. 몇 가지 특징을 살펴보자.

- 열거 타입 상수의 값으로 집합을 구성한다.
- java.util 패키지 안에 속해 있다.
- Set 인터페이스를 완벽히 구현한다.
- 타입 안전하다.
- 다른 어떤 Set 구현체와도 함께 사용할 수 있다.
- EnumSet의 내부는 비트 벡터로 구현되어 있다. 원소가 64개 이하이면, EnumSet 전체를 long 변수 하나로 표현하여 비트 필드와 비슷한 성능을 보인다.

다음은 비트 필드 방식의 예를 EnumSet으로 수정해본 코드다.

```java
import javaj.util.Set;

public class Text {
    public enum Style {
        BOLD, ITALIC, UNDERLINE, STRIKETHROUGH
    }
    
    // 어떤 Set을 넘겨도 되나, EnumSet이 가장 좋다.
    public void applyStyles(Set<Style> styles) {
        ...
    }
}
```

applyStyles 메서드에 EnumSet을 넘겨보자.

```java
text.applyStyles(EnumSet.of(Style.BOLD, Style.ITALIC));
```

applyStyles 메서드의 매개변수가 `EnumSet<Style>`이 아니라 `Set<Style>`인 이유는, 이왕이면 인터페이스로 받는 게 좋기 때문이다(아이템 64). 인터페이스로 선언하였기 때문에 어떤 Set 구현체도 받아들일 수 있게 되었다.


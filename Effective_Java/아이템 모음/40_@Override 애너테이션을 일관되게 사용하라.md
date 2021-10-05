# 6장. 열거 타입과 애너테이션(p207~252)

## 아이템 40. @Override 애너테이션을 일관되게 사용하라(p246~248)

@Override는 메서드 선언에만 달 수 있는 애너테이션으로, 상위 타입의 메서드를 재정의하기 위해 사용된다. 다음의 Bigram 프로그램 예시를 살펴보자. 바이그램은 알파벳 2개로 이루어진 문자열을 나타낸다.

```java
import java.util.HashSet;
import java.util.Set;

public class Bigram {
    private final char first;
    private final char second;

    public Bigram(char first, char second) {
        this.first = first;
        this.second = second;
    }

    public boolean equals(Bigram b) {
        return b.first == first && b.second == second;
    }

    public int hashCode() {
        return 31 * first + second;
    }

    public static void main(String[] args) {
        Set<Bigram> s = new HashSet<>();
        for (int i = 0; i < 10; i++) {
            for (char ch = 'a'; ch <= 'z'; ch++) {
                s.add(new Bigram(ch, ch));
            }
        }
        System.out.println(s.size()); // 260
    }
}
```

main 메서드에서는 a부터 z까지 바이그램 26개가 10번씩 생성되어 집합 s에 추가된다. 집합의 특성상 같은 알파벳을 필드로 가지는 바이그램들은 모두 중복 처리되어 최종 크기가 26이 출력되어야 하지만 실제 결과는 260이 출력되고 있다.

어디서부터 문제가 시작되었을까? 위의 equals 메서드를 다시 한 번 살펴보자. 자세히 보면 equals 메서드는 '재정의(overriding)'가 아닌 '다중정의(overloading, 아이템 52)'가 되어 있다. Object의 equals를 재정의하려면 매개변수 타입을 Object로 선언하는 게 맞다. 하지만 여기에서는 Object가 아닌 Bigram을 선택하였다. 그래서 Object의 equals 메서드와는 별개인 새로운 equals가 정의되었다. Object의 equals는 == 연산자처럼 객체 식별성(identity)만을 확인한다. 이에 따라 결과적으로 같은 멤버를 가지는 바이그램들이 중복 없이 하나하나 계수되어 260이 출력되었다.

```java
Bigram bigram1 = new Bigram('a', 'a');
Bigram bigram2 = new Bigram('a', 'a');

System.out.println(bigram1 == bigram2); // false
System.out.println(Objects.equals(bigram1, bigram2)); // false
```

이번에는 의도한 결과를 내기 위해 equals에 @Override 애너테이션을 붙이고 코드를 살짝 수정해보자.

```java
@Override
public boolean equals(Object o) {
    if (!(o instanceof Bigram))
        return false;
    Bigram b = (Bigram) o;
    return b.first == first && b.second == second;
}
```

이제 다시 위의 main 메서드를 돌리면 26이 출력되는 걸 볼 수 있다.

```java
System.out.println(s.size()); // 26
```

## 마무리

- **상위 클래스의 메서드를 재정의하려는 모든 메서드에 @Override 애너테이션을 달자.**
- **예외는 오직 한 가지뿐이다.** 구체 클래스에서 상위 클래스의 **추상 메서드**를 재정의할 때는 굳이 @Override를 달지 않아도 된다(물론, 다른 재정의 메서드와의 일관성을 지키기 위해 달아도 된다).
- IDE는 @Override를 일관되게 사용하는 데 도움을 준다. IDE에서 어떤 경고 메시지를 내보내는지 잘 체크하자.
- 참고로, **@Override는 클래스뿐 아니라 인터페이스의 메서드를 재정의할 때도 사용하자.** 애너테이션을 사용하면 컴파일시 올바른 메서드 시그니처인지 검증할 수 있다.


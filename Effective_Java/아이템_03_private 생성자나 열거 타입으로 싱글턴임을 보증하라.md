# 2장. 객체 생성과 파괴(p7~94)

## 아이템 3. private 생성자나 열거 타입으로 싱글턴임을 보증하라(p23~25)

> 싱글턴(singleton)이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다. 함수와 같은 무상태(stateless) 객체나 설계상 유일해야 하는 시스템 컴포넌트가 그 예다.

**싱글턴을 만드는 방식**은 세 가지가 있다.

## 1. public static 필드 방식

첫 번째 방식은 **public static 필드**(멤버 변수)를 사용하는 것이다. 클래스에 **private 생성자**를 만들어 외부에서 객체를 생성하는 것을 차단하고, **내부에서 객체**를 만들어 public static 필드에 담으면 된다. 이때 멤버 변수는 외부에서 인스턴스에 접근할 수 있는 **유일한 수단**이 된다. 예를 들면 다음과 같다.

```java
/**
 * Mj (Michael Jackson)
 * 마이클 잭슨은 지구 상에서 유일한 존재다.
 */
public class Mj {
    public static final Mj INSTANCE = new Mj(); // final 키워드를 사용하여 재할당을 막는다.
    
    /** 
     * private 생성자는 Mj.INSTANCE를 초기화할 때 딱 한 번 호출된다.
     */
    private Mj() { ... }
    
    public void moonwalk() { ... }
}
```

이렇게 하면 리플렉션 API인 AccessibleObject.setAccessible을 사용해 private 생성자를 호출하지 않는 한 Mj의 싱글턴 인스턴스를 사용할 수 있다. 리플렉션을 방어하고자 한다면 생성자에서 두 번째 객체가 생성될 때 예외를 던지게 하면 된다.

## public static 필드 사용의 장점

public static 필드를 사용하는 장점은 다음과 같다.

1. **해당 클래스가 싱글턴임이 API에 명확히 드러난다.** 생성자를 외부에서 사용할 수 없고(private하므로), 객체를 담는 INSTANCE 필드는 **final 키워드**를 사용한다. final 키워드를 사용할 때 필드에 재할당이 불가능하므로 해당 클래스가 싱글턴으로 사용됨을 알 수 있다.
2.  한 줄의 구현으로 싱글턴 인스턴스를 구현할 수 있어 **매우 간결**하다.

## 2. 정적 팩터리 메서드 방식

두 번째 방식은 **public static 멤버 메서드**(정적 팩터리 메서드)를 사용하는 것이다. 앞에서 필드를 사용한 예시를 메서드를 사용하는 방식으로 바꾸어보겠다.

```java
public class Mj {
    private static final Mj INSTANCE = new Mj();
    
    private Mj() { ... }
    
    public static Mj getInstance() { 
    	return INSTANCE:
    }
    
    public void mooonwalk() { ... }
}
```

## 정적 팩터리 메서드 사용의 장점

1. 정적 팩터리 메서드는 **API를 유지한 채 싱글턴에서 벗어날 수 있다.** 예를 들어 호출하는 스레드별로 다른 인스턴스를 넘겨줄 수 있다.
2. 정적 팩터리 메서드에 **제네릭**을 추가할 수 있다.
3. 정적 팩터리 메서드 참조를 **공급자(supplier)**로 사용할 수 있다. 예를 들어 `Mj::getInstance`를 `Supplier<Mj>`로 사용할 수 있다. 

위와 같은 장점이 필요하지 않다면 public 멤버 변수를 사용하는 게 좋다.

위의 두 가지 방식의 경우, Serializable을 구현한다는 선언으로는 싱글턴 클래스가 직렬화되지 않는다. 

> 모든 인스턴스 필드를 일시적(transient)이라고 선언하고 readResolve 메서드를 제공해야 한다. 이렇게 하지 않으면 직렬화된 인스턴스를 역직렬화할 때마다 새로운 인스턴스가 만들어진다. 가짜 Mj 탄생을 예방하려면 Mj 클래스에 다음의 readResolve 메서드를 추가하자.
>
> ***readResolve 메서드** : serializable 하지 않은 객체를 field로 가진 경우, field를 serialize 해주기 위해 필요하다.

```java
// 싱글턴임을 보장해주는 readResolve 메서드
private Object readResolve() {
    // '진짜' Mj를 반환하고, 가짜 Mj는 가비지 컬렉터에 맡긴다.
    return INSTANCE;
}
```

## 3.  열거 타입

세 번째 방식은 **열거(Enum) 타입**을 사용하는 것이다.

```java
public enum Mj {
    INSTANCE;
    
    public void moonwalk() { ... }
}
```

## 열거 타입 사용의 장점

1. public 필드 방식보다 **훨씬 간결**하다.
2. 아주 복잡한 **직렬화** 상황이나 **리플렉션 공격**에서도 제2의 인스턴스가 생기는 일을 완벽히 막아준다. 

> **대부분의 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다.**


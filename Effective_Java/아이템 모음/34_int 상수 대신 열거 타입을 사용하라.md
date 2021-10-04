# 6장. 열거 타입과 애너테이션(p207~252)

> 자바에는 특수한 목적의 참조 타입이 두 가지가 있다. 하나는 클래스의 일종인 **열거 타입**(enum: 열거형)이고, 다른 하나는 인터페이스의 일종인 **애너테이션**(annotation)이다. 이번 장에서는 이 타입들을 올바르게 사용하는 방법을 알아본다.

## 아이템 34. int 상수 대신 열거 타입을 사용하라(p208~220)

## 정수 열거 패턴의 단점

정수 열거 패턴(int enum pattern)은 정수 타입의 상수를 선언하고, 이를 사용하는 방식을 말한다. 이에 대한 예시는 다음과 같다.

```java
public static final int APPLE_FUJI = 0;
public static final int APPLE_PIPPIN = 1;
public static final int APPLE_GRANNY_SMITH = 2;

public static final int ORANGE_NAVEL = 0;
public static final int ORANGE_TEMPLE = 0;
public static final int ORANGE_BLOOD = 0;
```

사실 정수 열거 패턴 방식에는 단점이 많다. 

- **단점 1**: **타입 안전하지 않다.** ORANGE_NAVEL을 사용할 자리에 APPLE_PIPPIN을 사용해도 컴파일하는 데 아무 문제가 없다. 만약 메서드의 매개변수로 사과 타입만 넣어야 하는데 오렌지 타입을 넣을 수 있다면, 이는 타입 안전하지 않은 것이다. 
- **단점 2**: **동의어를 구분하기 위해 불필요한 접두어를 사용해야 할지도 모른다.** 예를 들어, mercury라는 단어는 수은(원소)과 수성(행성)의 동의어다. 이들을 구분하기 위해 각각 ELEMENT_MERCURY와 PLANET_MERCURY의 방식으로 MERCURY 앞에 접두어를 붙여주어야 한다.
- **단점 3**: **상수 값을 바꿀 때마다 반드시 컴파일해야 한다.** 이들은 평범한 상수이기 때문에 컴파일한 값이 그대로 파일에 새겨진다. 따라서 값을 바꾸고 컴파일하지 않으면 사용자는 의도하지 않은 값을 사용할 수 있다. 

- **단점 4**: **정수 상수는 문자열로 출력하기에 좋지 않다.** 출력할 때나 로깅할 때 의미가 아닌 숫자 값이 보이기 때문이다.

## 열거 타입   

> **열거 타입은 일정 개수의 상수 값을 정의한 다음, 그 외의 값은 허용하지 않는 타입**이다.
>
> 열거 타입 자체가 **클래스**이며, 상수 하나당 자신의 인스턴스를 하나씩 만들어 public static final 필드로 공개한다.
>
> 열거 타입은 **외부 생성자를 제공하지 않으므로** 사실상 final이며, 딱 하나씩만 존재한다(싱글턴이자 인스턴스 통제 형태).

열거 타입의 예는 다음과 같다.

```java
public enum Apple { FUJI, PIPPIN, BRANNY_SMITH }
public enum Orange { NAVEL, TEMPLE, BLOOD }
```

## 열거 타입의 장점

- **장점 1**: **컴파일타임 타입 안전성을 제공한다.** 만약 Apple 열거 타입을 매개변수로 받는 메서드가 있다면, 건네 받은 참조는 (null이 아니면) Apple의 세 가지 값 중 하나임이 확실하다. 이때 다른 타입의 값을 넘기려 하면 컴파일 오류가 발생한다.
- **장점 2**: **상수마다 네임스페이스를 가지기 때문에 중복이 없다**(같은 열거 타입이 아니면).
- **장점 3**: **새로운 상수를 추가하거나 순서를 바꿔도 다시 컴파일하지 않아도 된다.** 열거 타입은 기본적으로 필드의 이름만 공개하기 때문에 상수 값은 클라이언트 파일에 새겨지지 않는다. 
- **장점 4**: **toString 메서드를 사용하면 유용한 문자열을 출력할 수 있다.**
- **장점 5**: **임의의 메서드나 필드를 추가할 수 있다.**
- **장점 6**: **임의의 인터페이스를 구현할 수 있다.**
- **장점 7**: **Object 메서드들(equals, hashCode, toString)이 높은 품질로 구현되어 있다.**
- **장점 8**: **Comparable(아이템 14)과 Serializable(12장)을 구현하며, 직렬화 형태에 어느 정도 변형을 가해도 문제 없이 동작한다.**

## 열거 타입의 필드와 메서드

열거 타입 상수와 함께 사용하고 싶은 데이터가 있다고 해보자. 어느 정도로 함께 사용하고 싶냐면 상수를 사용할 때 그 옆에서 바로 쓰고 싶을 정도이다. 잘 생각해보면 이러한 개념은 클래스의 멤버 변수 또는 메서드에 가깝다.

열거 타입 역시 클래스이므로 멤버 변수(필드) 또는 메서드를 가진다. 예를 들어 Apple과 Orange가 열거 타입이라면, 자기 자신의 색을 알려주거나 이미지를 반환하는 메서드를 가질 수 있다.

태양계의 행성들도 열거 타입의 좋은 예시다. 행성마다 질량과 반지름을 가지고, 이를 이용해 표면중력을 계산할 수 있다.

```java
public enum Planet {
    MERCURY(3.302e+23, 2.439e6),
    VENUS(4.869e+24, 6.052e6),
    EARTH(5.975e+24, 6.378e6),
    MARS(6.419e+23, 3.393e6),
    JUPITER(1.899e+27, 7.149e7),
    SATURN(5.685e+26, 6.027e7),
    URANUM(8.683e+25, 2.556e7),
    NEPTUNE(1.024+26, 2.477e7);

    private final double mass; // 질량(단위: 킬로그램)
    private final double radius; // 반지름(단위: 미터)
    private final double surfaceGravity; // 표면중력(단위: m / s^2)

    // 중력상수(단위: m^3 / kg s^2)
    private static final double G = 6.67300E-11;

    // 생성자
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
        this.surfaceGravity = G * mass / (radius * radius);
    }

    public double mass() { return mass; }
    public double radius() { return radius; }
    public double surfaceGravity() { return surfaceGravity; }

    public double surfaceWeight(double mass) {
        return mass * surfaceGravity; // F = ma
    }
}
```

열거 타입 오른쪽 괄호 안의 숫자는 생성자의 매개변수다. 여기에서는 각각 행성의 질량과 반지름을 의미한다. 

**열거 타입은 근본적으로 불변이기 때문에 모든 필드는 final이어야 한다(아이템 17).** 또한 **접근 수준을 private으로 낮추고 별도의 public 접근자 메서드를 두고 사용하는 게 좋다(아이템 16).**

## 상수마다 다른 동작을 사용할 경우

상수에 더 다양한 기능을 제공해보자. 예를 들어 사친연산 계산기에 선언된 열거 타입 상수(연산 종류)가 실제 연산 기능을 가지고 수행한다고 해보자.

```java
public enum Operation {
    PLUS, MINUS, TIMES, DIVIDE;
    
    // 상수가 뜻하는 연산을 수행한다.
    public double apply(double x, double y) {
        switch (this) {
            case PLUS: return x + y;
            case MINUS: return x - y;
            case TIMES: return x * y;
            case DIVIDE: return x / y;
        }
        throw new AssertionError("알 수 없는 연산: " + this);
    }
}
```

이와 같이 작업한 코드에는 분명히 단점이 있다. 새로운 상수를 추가할 때 case 문을 깜빡하면 추가한 상수 연산을 수행할 때 런타임 오류가 발생한다.

이 방법을 대신하여 상수별로 동작이 다른 코드를 구현해보자.

```java
public enum Operation {
    PLUS {public double apply(double x, double y) {return x + y;}}, 
    MINUS {public double apply(double x, double y) {return x - y;}}, 
    TIMES {public double apply(double x, double y) {return x * y;}}, 
    DIVIDE {public double apply(double x, double y) {return x / y;}}; 

    public abstract double apply(double x, double y);
}
```

방법은 열거 타입에 추상 메서드 apply를 선언하고 상수별로 클래스 몸체(constant-specific class body)를 재정의하는 것이다. 이를 **상수별 메서드**(constant-specific method implementation)이라 한다. 만약 새로 추가한 상수에 apply를 재정의하지 않으면 컴파일 오류가 발생할 것이다.

다음처럼 상수별 메서드 구현을 상수별 데이터와 결합할 수도 있다.

```java
public enum Operation {
    PLUS("+") {
        public double apply(double x, double y) {return x + y;}
    },
    MINUS("-") {
        public double apply(double x, double y) {return x - y;}
    },
    TIMES("*") {
        public double apply(double x, double y) {return x * y;}
    },
    DIVIDE("/") {
        public double apply(double x, double y) {return x / y;}
    };

    private final String symbol;

    Operation(String symbol) { this.symbol = symbol; }
	
    // toString을 재정의해 해당 연산을 뜻하는 기호를 반환한다.
    @Override
    public String toString() { return symbol; }

    public abstract double apply(double x, double y);
}
```

다음은 위에서 정의한 열거 타입의 필드와 메서드들을 편리하게 사용한 예이다.

```java
public static void main(String[] args) {
    double x = Double.parseDouble(args[0]);
    double y = Double.parseDouble(args[1]);
    for (Operation op : Operation.values()) {
        System.out.printf("%f %s %f = %f%n", x, op, y, op.apply(x, y));
    }
}
```

명령줄 인수에 2와 4를 입력하면 다음 결과를 출력한다.

```JAVA
2.000000 + 4.000000 = 6.000000
2.000000 - 4.000000 = -2.000000
2.000000 * 4.000000 = 8.000000
2.000000 / 4.000000 = 0.500000
```

## valueOf 메서드

열거 타입에는 valueOf(String) 메서드가 내장되어 있다. 즉, 상수 이름을 입력받았을 때 그 이름에 해당하는 열거 타입 상수를 반환한다.

```java
// 아래 두 가지 방법 중 어느 것을 사용해도 된다.
Operation plus = Operation.valueOf("PLUS"); // 매개변수: String name
Operation minus = Operation.valueOf(Operation.class, "MINUS");  // 매개변수: Class<T> enumType, String name

System.out.println(plus.getClass());  // class Operation$1 (PLUS가 첫 번째 상수라서 1)
System.out.println(minus.getClass());  // class Operation$2 (MINUS가 첫 번째 상수라서 1)
```

## fromString 메서드

한편, toString의 반환값을 상수의 값으로 재정의했다면, 이와 반대되는 fromString 메서드 지원도 고려해보자. fromString 메서드는 문자열을 인수를 받아 이를 toString의 반환값으로 가지는 열거 타입 상수를 반환한다.

```java
// 열거 타입 Operation 안에서 작성한다.

private static final Map<String, Operation> stringToEnum =
    Stream.of(values()).collect(
    toMap(Object::toString, e -> e));

// 지정한 문자열에 해당하는 Operation을 (존재한다면) 반환한다.
public static Optional<Operation> fromString(String symbol) {
    return Optional.ofNullable(stringToEnum.get(symbol));
}
```

위의 코드는 아래와 같이 사용될 수 있다.

```java
Optional<Operation> operation = Operation.fromString("/");

System.out.println(operation.isEmpty() ? null : operation.get().getClass()); // class Operation$4
```

## '전략' 열거 타입

전략 열거 타입은 열거 타입의 멤버로 전략을 더하는 방식이다. 열거 타입 상수끼리 **같은 동작의 코드를 공유하고 싶을 때** 사용한다.

예를 들어 한 급여명세서는 요일별 상수를 가지고, 각 요일별 근무 수당을 계산해서 사용하고 싶다고 해보자. 계산 메서드는 직원이 일한 시간(분)만큼 기본 수당을 계산하고, 초과근무 시간만큼 잔업수당을 구해 이 둘을 합산하여 반환한다.

다음은 위와 같은 경우 바로 전략 열거 타입을 사용한 코드 예시다. 

```java
enum PayrollDay {
    MONDAY(WEEKDAY), TUESDAY(WEEKDAY), WEDNESDAY(WEEKDAY),
    THURSDAY(WEEKDAY), FRIDAY(WEEKDAY),
    SATURDAY(WEEKEND), SUNDAY(WEEKEND);

    private final PayType payType;

    PayrollDay(PayType payType) {
        this.payType = payType;
    }

    int pay(int minutesWorked, int payRate) {
        return payType.pay(minutesWorked, payRate);
    }

    // 전략 열거 타입
    enum PayType {
        WEEKDAY {
            int overtimePay(int minsWorked, int payRate) {
                return minsWorked <= MINS_PER_SHIFT ? 0 :
                (minsWorked - MINS_PER_SHIFT) * payRate / 2;
            }
        },
        WEEKEND {
            int overtimePay(int minsWorked, int payRate) {
                return minsWorked * payRate / 2;
            }
        };

        abstract int overtimePay(int mins, int payRate);

        private static final int MINS_PER_SHIFT = 8 * 60;

        int pay(int minsWorked, int payRate) {
            int basePay = minsWorked * payRate;
            return basePay + overtimePay(minsWorked, payRate);
        }
    }
}
```

> 대부분의 경우 열거 타입의 성능은 정수 상수와 별반 다르지 않다. 열거 타입을 메모리에 올리는 공간과 초기화하는 시간이 들긴 하지만 체감될 정도는 아니다.

## 

**필요한 원소를 컴파일타임에 다 알 수 있는 상수 집합이라면 항상 열거 타입을 사용하자.**

**열거 타입에 정의된 상수 개수는 영원하지 않다.** 열거 타입은 나중에 상수가 추가돼도 바이너리 수준에서 호환되도록 설계되었다.


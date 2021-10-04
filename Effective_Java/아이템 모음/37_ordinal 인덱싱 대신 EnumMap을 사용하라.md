# 6장. 열거 타입과 애너테이션(p207~252)

## 아이템 37. ordinal 인덱싱 대신 EnumMap을 사용하라(p226~231)

가끔 ordinal 메서드(아이템 35)를 사용하여 배열 또는 리스트 원소의 인덱스를 얻는 경우가 있다. 상황을 보기 전에 먼저 간단한 Plant 코드를 살펴보자.

```java
class Plant {
    enum LifeCycle { ANNUAL, PERENNIAL, BIENNIAL }
    
    final String name;
    final LifeCycle lifeCycle;

    Plant(String name, LifeCycle lifeCycle) {
        this.name = name;
        this.lifeCycle = lifeCycle;
    }
    
    @Override
    public String toString() {
        return name;
    }
}
```

만약 여기서 ordinal 값을 사용해 정원(garden)의 식물들을 생애주기(한해살이, 여러해살이, 두해살이)별로 묶는다고 하면 다음의 코드처럼 작성할 수 있을 것이다.

```java
Set<Plant>[] plantsByLifeCycle = (Set<Plant>[]) new Set[LifeCycle.values().length];

for (int i = 0; i < plantsByLifeCycle.length; i++) {
    plantsByLifeCycle[i] = new HashSet<>();
}

for (Plant p : garden) {
    plantsByLifeCycle[p.lifeCycle.ordianl()].add(p);
}

// 결과 출력
for (int i = 0; i < plantsByLifeCycle.length; i++) {
    System.out.printf("%s: %s%n",
                      Plant.LifeCycle.values()[i], plantsByLifeCycle[i]);
}
```

위 코드의 문제는 무엇일까?

- 먼저, 배열은 제네릭과 호환되지 않기 때문에(아이템 28) 비검사 형변환을 수행해야 한다.
- 배열 인덱스의 의미를 모르고, 새로 추가할 때도 마찬가지로 모르기 때문에 결과를 출력할 때마다 레이블을 달아주어야 한다.
- 정수는 열거 타입과 달리 타입 안전하지 않다. 잘못된 값을 사용하면 ArrayIndexOutOfBoundsException 오류를 만날 수 있다.

## EnumMap 방식

EnumMap 클래스 사용은 이러한 문제들을 해결해준다. 위의 배열의 역할은 인덱스와 열거 타입 상수를 매핑하는 것이었다. EnumMap을 사용하여 Map으로 이를 대체해도 무방하다. 다음은 EnumMap을 사용하여 수정한 코드다.

```java
Map<LifeCycle, Set<Plant>> plantsByLifeCycle = new EnumMap<>(Plant.LifeCycle.class);

for (Plant.LifeCycle lc : Plant.LifeCycle.values()) {
    plantsByLifeCycle.put(lc, new HashSet<>());
}
for (Plant p : garden) {
    plantsByLifeCycle.get(p.lifeCycle).add(p);
}

System.out.println(plantsByLifeCycle);
```

위의 코드에서 EnumMap을 사용함으로써 다음의 이점이 생겼다.

- 더 짧고 안전해졌다. 안전하지 않은 형변환을 하지 않아도 된다.
- 성능도 기존의 성능과 비슷하다. EnumMap의 내부에서 배열을 사용하기 때문이다.
- 결과를 출력할 때마다 레이블을 달아주지 않아도 된다. 맵의 키인 열거 타입이 출력시 상수 문자열을 제공하기 때문이다. 
- 인덱스 오류 문제는 애초에 발생하지 않는다.
- EnumMap 생성자의 매개변수인, 키 타입의 Class 객체는 한정적 타입 토큰이다. 이는 런타임에 제네릭 타입 정보를 제공하게 해준다(아이템 33).

만약 스트림(아이템 45)을 사용한다면 코드를 다음처럼 더 줄일 수도 있다.

```java
System.out.println(Arrays.stram(garden)
                   .collect(groupingBy(p -> p.lifeCycle,
                                       () -> new EnumMap<>(LifeCycle.class), toSet())));
```

> 매개변수 3개짜리 Collectors.groupingBy 메서드는 mapFactory 매개변수에 원하는 맵 구현체를 명시해 호출할 수 있다.

## 마무리

- **배열의 인덱스를 얻기 위해 ordinal을 쓰는 것은 일반적으로 좋지 않으니, 대신 EnumMap을 사용하라.**

- (참고) 다차원 관계는 EnumMap<..., EnumMap<...>>으로 표현하라. 자세한 예시는 책을 참고하라.

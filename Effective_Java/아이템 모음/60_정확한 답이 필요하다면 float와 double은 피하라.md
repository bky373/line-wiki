# 9장. 일반적인 프로그래밍 원칙(p343~384)

> 이번 장에서는 자바 언어의 핵심 요소라고 할 수 있는 다음 내용들을 다룬다.
>
> - 지역변수
> - 제어구조
> - 라이브러리
> - 데이터 타입 
> - 리플렉션과(언어 경계를 넘나드는 기능) 
> - 네이티브 메서드
> - 최적화와 명명 규칙

## 아이템 60. 정확한 답이 필요하다면 float와 double은 피하라(p355~357)

**float와 double 타입은 정확한 숫자 값이 필요할 때는 사용하면 안 된다.** 특히 **금융 관련 계산에서 사용하면 안 된다.** 0.1 이나 10의 음의 거듭 제곱 수(10^(-1), 10^(-2) 등)를 표현할 수 없기 때문이다. 이들은 과학과 공학 계산용으로 이진 부동소수점 연산을 위해 설계되었다. 넓은 범위의 수를 빠르게 정밀한 '근사치'로 계산할 때 필요한 것이다. 그렇다면 구체적으로 왜 금융 계산에서 이들을 사용하면 안 되는지 다음 예제를 통해 살펴보겠다.

가지고 있는 1.03달러 중에서 42센트를 사용하였고, 이제 남은 돈을 아래처럼 출력하고자 한다. 결과는 어떠할까?

```java
System.out.println(1.03 - 0.42);
```

실제로 코드를 실행하면 예상과 다르게 0.6100000000000001이 출력된다. 이와 같은 문제는 다음 사례에서도 이어진다.

10센트짜리 사탕 9개를 1달러를 주고 샀다고 해보자. 남은 돈은 얼마인가? 

```java
System.out.println(1.00 - 9 * 0.10);
```

위 코드는 0.09999999999999998을 출력한다. 반올림을 한다고 해도 궁긍적으로 문제를 해결하는 건 아니다.

위 문제를 올바로 해결하기 위해선 타입을 변경해야 한다. 즉, **금융 계산에서는 BigDecimal, int 혹은 long 타입을 사용해야 한다.**

```java
System.out.println(new BigDecimal("1.00").subtract(BigDecimal.valueOf(9 * 0.10)));
```

BigDecimal로 바꾸어 다시 코드를 실행하면 정확한 결괏값 0.10이 정상적으로 출력된다. 이처럼 BigDecimal을 사용하면 계산을 정확히 할 수 있다는 장점이 있다. 하지만 기본 타입보다 사용하기 훨씬 불편하고, 훨씬 느리기 때문에 아쉬운 것도 사실이다. 

Bigdecimal의 대안으로 int 혹은 long 타입을 사용할 수도 있다. 코드를 간결하게 만들어주지만, 값의 크기가 제한적이고 소수점을 직접 관리해야 하므로, 항상 상황에 맞는 타입을 선택하도록 하자.

## 마무리

- 정확한 답이 필요한 계산에는 float, double을 피하자.
- 만약 코딩 시의 불편함이나 성능 저하를 감수하고 소수점 추적을 시스템에 맡길 용의가 있다면, 정확한 계산을 위해 BigDecimal을 사용하자. 참고로, BigDecimal의 8가지 반올림 모드를 이용하면 반올림을 완벽히 제어할 수 있다.
- 반면, 성능이 중요하고 소수점을 직접 추적할 수 있고 숫자가 너무 크지 않다면 int, long을 사용하자.
- 숫자를 아홉 자리 십진수로 표현할 수 있다면 int를 사용하고, 열여덟 자리 십진수로 표현할 수 있다면 long을 사용하자. 열여덟 자리를 넘어가면
  BigDecimal을 사용해야 한다. 



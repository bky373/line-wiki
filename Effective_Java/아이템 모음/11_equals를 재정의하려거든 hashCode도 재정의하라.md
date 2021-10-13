# 3장. 모든 객체의 공통 메서드(p51~94)

## 아이템 11. equals를 재정의하려거든 hashCode도 재정의하라(p67~72)

**equals를 재정의한 클래스는 반드시 hashCode도 재정의해야 한다.** 그렇지 않으면 인스턴스를 HashMap이나 HashSet 같은 컬렉션의 원소로 사용할 때 문제가 발생할 수 있다.

## Object 명세에서 발췌한 규약

- **equals 비교에 사용되는 정보가 변경되지 않았다면,** 애플리케이션이 실행되는 동안 그 객체의 **hashCode 메서드는 몇 번을 호출해도 일관되게 항상 같은 값을 반환해야 한다.** 단, 애플리케이션을 다시 실행한다면 이 값이 달라져도 상관없다.
- **equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다.**
- equals(Object)가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 단, 다른 객체에 대해서는 다른 값을 반환해야 해시테이블의 성능이 좋아진다.

## 좋은 hashCode를 작성하는 요령

1. **int 변수 result를 선언한 후 값 c로 초기화한다.** 

   > 이때 c는 해당 객체의 첫 번째 핵심 필드를 단계 2.a 방식으로 계산한 해시코드다(여기서 핵심 필드란 equals 비교에 사용되는 필드를 말한다. 아이템 10 참조)

2. **해당 객체의 나머지 핵심 필드 f 각각에 대해 다음 작업을 수행한다.**

   a. **해당 필드의 해시코드 c를 계산한다.**

   - 기본 타입 필드라면, Type.hashCode(f)를 수행한다. 여기서 Type은 해당 기본 타입의 박싱 클래스다.
   - 참조 타입 필드라면, 이 필드의 표준형(canonical representation)을 만들어 그 표준형의 hashCode를 호출한다. 필드의 값이 null이면 0을 사용한다.
   - 필드가 배열이라면, 핵심 원소 각각을 별도 필드처럼 다룬다. 배열에 핵심 원소가 하나도 없다면 단순히 상수(0을 추천한다)를 사용한다. 모든 원소가 핵심 원소라면 Arrays.hashCode를 사용한다.

   b. **단계 2.a에서 계산한 해시코드 c로 result를 갱신한다. 코드로는 다음과 같다.** 곱하는 숫자를 31로 정한 이유는 31이 홀수이면서 소수(prime)이기 때문이다. 

   ```java
   result = 31 * result + c;
   ```

3. **result를 반환한다.**

이 요령을 PhoneNumber 클래스에 적용해보자.

```java
@Override 
public int hashCode() {
    int result = Short.hashCode(areaCode);
    result = 31 * result + Short.hashCode(prefix);
    result = 31 * result + Short.hashCode(lineNum);
    return result;
}
```

Objects 클래스가 제공하는 hash 메서드 보다 hashCode 작성 요령에 따라 직접 만든 hashCode가 성능상 이점이 많다. 단, **hashCode가 반환하는 값의 생성 규칙을 API 에 구체적으로 설명하지는 말자.** 클라이언트가 최대한 값에 의존하지 않고 hashCode를 사용/변경 할 수 있도록 하기 위함이다.


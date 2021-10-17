# 8장. 메서드(p297~342)

> 이번 장에서는 **메서드를 설계할 때 주의할 점**들을 살펴본다. 
>
> - **매개변수**와 **반환값**을 어떻게 처리해야 하는지
> - **메서드 시그니처**는 어떻게 설계해야 하는지
> - **문서화**는 어떻게 해야 하는지를 다룬다. 
> - 이번 장의 내용 중 상당 부분은 메서드뿐 아니라 **생성자**에도 적용된다. 

## 아이템 56. 공개된 API 요소에는 항상 문서화 주석을 작성하라(p332~342)

자바에서는 자바독(Javadoc)을 사용해 API 문서를 작성할 수 있다. 자바독은 소스코드 파일에서 문서화 주석(doc comment; 자바독 주석)이라는 형태로 기술된 설명을 추려 API 문서로 변환해준다. 

문서화 주석을 작성하는 규칙은 [How to Write Doc Comments](https://www.oracle.com/kr/technical-resources/articles/java/javadoc-tool.html)을 참고하자(자바 4 이후로 갱신되지는 않았지만 가치가 있다).

**API를 올바로 문서화하려면 공개된 모든 클래스, 인터페이스, 메서드, 필드 선언에 문서화 주석을 달아야 한다.** 직렬화할 수 있는 클래스라면 직렬화 형태(아이템 87)에 관해서도 적어야 한다. 기본 생성자에는 문서화 주석을 달 방법이 없으니 공개 클래스는 절대 기본 생성자를 사용하면 안 된다.

메서드용 문서화 주석에는 해당 메서드와 클라이언트 사이의 규약을 명료하게 기술해야 한다. **메서드가 어떻게 동작하는지가 아니라 무엇을 하는지를 기술해야 한다.** 즉, how가 아닌 what을 기술해야 한다. 또한 클라이언트가 해당 메서드를 호출하기 위한 전제조건(precondition)과 메서드가 성공한 후 만족해야 하는 사후조건(postcondition)을 모두 나열해야 한다. 일반적으로 @throws 태그로 비검사 예외를 선언함으로써 전제조건을 암시적으로 기술한다. 만약 시스템 상태에 변화를 가져오는 부작용이 예상되면 이 또한 문서에 남겨야 한다.

> 메서드의 계약(contract)을 완벽히 기술하려면. 
>
> - 모든 매개변수에 @param 태그를, 
> - 반환 타입이 void가 아니라면 @return 태그를, 
> - 발생할 가능성이 있는 (검사든 비검사든) 모든 예외에 @throws 태그를(아이템 74) 달아야 한다.
>
> 관례상 **@param 태그와 @return 태그의 설명은 해당 매개변수가 뜻하는 값이나 반환값을 설명하는 명사구**를 쓴다(BigInteger의 API 문서 참고).
>
> 관례상 @param, @return, @throws 태그의 설명에는 마침표를 붙이지 않는다.

위의 규칙을 모두 반영한 문서화 주석의 예는 다음과 같다.

```java
/**
 *
 * Returns the element at the specified position in this list.
 * <p>This method is <i>not</i> guaranteed to run in constant
 * time. In some implementations it may run in time proportional
 * to the element position.
 *
 * @param index index of element to return; must be
non-negative and less than the size of this list
* @return the element at the specified position in this list
* @throws IndexOutOfBoundsException if the index is out of range ({@code index < 0 || index >= this.size()})
*/
E get(int index);
```

번역하면 다음과 같다.

```java
/**
* 이 리스트에서 지정한 위치의 원소를 반환한다.
*
* <p>이 메서드는 상수 시간에 수행됨을 보장하지 <i>않는다</i>. 구현에 따라
* 원소의 위치에 비례해 시간이 걸릴 수도 있다.
*
* @param index 반환할 원소의 인덱스; 0 이상이고 리스트 크기보다 작아야 한다.
* @return 이 리스트에서 지정한 위치의 원소
* @throws IndexOutOfBoundsException index가 범위를 벗어나면, 즉, ({@code index < 0 || index >= this.size()})이면 발생한다.
*/
E get(int index);
```

> (옮긴이) 설명을 한글로 작성할 경우 온전한 종결 어미로 끝나면 마침표를 써주는 게 일관돼 보인다.

자바독 유틸리티는 문서화 주석을 HTML로 변환한다. 문서화 주석에 사용된 HTML 태그(`<p>`와 `<i>`)들은 최종 HTML 문서에 반영된다.

@throws 절에서 {@code} 태그로 감싼 내용은 코드용 폰트로 렌더링한다. 그리고 이 내용에 포함된 HTML 요소나 다른 자바독 태그를 무시한다(덕분에 `<` 부등호를 그대로 사용할 수 있다). 

`<pre>`{@code ... 코드 ... }`</pre>` 형태로 쓰면 코드 예시를 여러 줄로 표현할 수 있다. 다른 기호는 무관하나 `@`기호에는 반드시 탈출문자를 붙여야 애너테이션으로 적용된다.

(영문) 문서화 주석에 쓰인 "this"는 호출된 메서드가 자리하는 객체를 가리킨다.

클래스를 상속용으로 설계하여 자기사용 패턴을 문서화해야 한다면 @impleSpec 태그를 사용하자(자바 8 추가). 

> 일반적인 문서화 주석은 해당 메서드와 클라이언트 사이의 계약을 설명한다. 반면, @implSpec 주석은 해당 메서드와 하위 클래스 사이의 계약을 설명한다.
>
> 하위 클래스들이 그 메서드를 상속하거나 super 키워드를 이용해 호출할 때 그 메서드가 어떻게 동작하는지를 명확히 인지하고 사용하도록 해줘야 한다.
>
> 다음은 사용 예시다.
>
> ```java
> /**
>  * Returns true if this collection is empty. .
>  * @implSpec
>  * This implementation returns {@code this.size() = 0}.
>  * @return true if this collection is empty
>  */
> public boolean isEmpty() { ... }
> ```
>
> 번역하면 다음과 같다.
>
> ```java
> /**
>  * 이 컬렉션이 비었다면 true를 반환한다.
>  *
>  * @implSpec
>  * 이 구현은 {@code this.size() == 0}의 결과를 반환한다.
>  * @return 이 컬렉션이 비었다면 true, 그렇지 않으면 false
>  */
> public boolean isEmpty() { ... }  
> ```
>
> 참고로, 자바 11까지 자바독 명령줄에서 `-tag "implSpec:a:Implementation Requirements:"` 스위치를 켜주지 않으면 @implSpec 태그를 무시해버린다.

{@literal} 태그를 사용하면 `<`, `>`, `&` 등의 HTML 메타문자를 넣을 수 있다. {@code} 태그와 비슷하지만 코드 폰트로 렌더링하지 않고, HTML 마크업이나 자바독 태그를 무시하게 해준다. 예를 들면 다음과 같이 사용된다.

```java
* A geometric series converges if {@literal |r| < 1}.
```

번역하면 다음과 같다.

```java
* {@literal |r| < 1}이면 기하 수열이 수렴한다.
```

문서화 주석의 첫 번째 문장은 해당 요소의 요약 설명(summary description)을 작성한다. 위의 예시 중 "이 리스트에서 지정한 위치의 원소를 반환한다.”라는 문장이 이를 의미한다. 이 설명은 고유해야 하기에, **한 클래스(혹은 인터페이스) 안에서 요약 설명이 똑같은 멤버(혹은 생성자)가 둘 이상 있으면 안 된다.** 다중정의된 메서드이더라도 같은 설명을 허용하지 않는다.

요약 설명이 끝나는 판단 기준은 처음 발견되는 {〈마침표〉 〈공백〉 〈다음 문장 시작〉} 패턴의 〈마침표〉까지다. 〈공백〉은 스페이스(space), 탭(tab), 줄바꿈(혹은 첫 번째 블록 태그)이며 〈다음 문장 시작〉은 '소문자가 아닌' 문자다. 이에 따르면 다음 주석에서 요약 설명은 "Mrs. "에서 끝난다.

```java
/**
 * A suspect, such as Colonel Mustard or Mrs. Peacock.
 */
public class Suspect { ... }
```

올바로 요약 설명을 마치려면 {@literal}로 감싸주자.

```java
/**
 * A suspect, such as Colonel Mustard or {@literal Mrs. Peacock).
 */
public class Suspect { ... }
```

자바 10부터는 {@summary}라는 요약 설명 전용 태그가 추가되어 보다 깔끔히 해결할 수 있다.

```java
/**
 * {@summary A suspect, such as Colonel Mustard or Mrs. Peacock}.
 */
public class Suspect { ... }
```

**메서드와 생성자의 요약 설명은 해당 메서드와 생성자의 동작을 설명하는 (주어가 없는) 동사구여야 한다.**

> 다음 예를 보자.
>
> - ArrayList(int initialCapacity): Constructs an empty list with the specified initial capacity
> - Collection.size(): Returns the number of elements in this collection.
>
> 번역하면 다음과 같다.
>
> - ArrayList(int initialCapacity) : 지정한 초기 용량을 갖는 빈 리스트를 생성한다.
> - Collection.size(): 이 컬렉션 안의 원소 개수를 반환한다.
>
> 영문의 경우, 2인칭 문장(return the number)이 아닌 3인칭 문장(returns the number)으로 써야 한다(한글 설명에서는 차이가 없다).

**클래스, 인터페이스, 필드의 요약 설명은 대상을 설명하는 명사절이어야 한다.**

> 다음 예를 보자.
>
> • Instant: An instantaneous point on the time-line.
> • Math.PI: The double value that is closer than any other to pi, the ratio of the circumference of a circle to its diameter.
>
> 번역하면 다음과 같다.
>
> • Instant: 타임라인상의 특정 순간(지점)
> • Math.PI: 원주율(pi)에 가장 가까운 double 값

{@index} 태그를 사용하면 API 용어를 색인화할 수 있다. 자바 9부터 자바독이 생성하는 HTML 문서에 검색(색인) 기능이 추가되어 유용하게 쓸 수 있다.

다음 예를 보자.

```java
* This method complies with the {@index IEEE 754} standard.
```

번역하면 다음과 같다.

```java
* 이 메서드는 {@index IEEE 754) 표준을 준수한다.
```

**제네릭 타입이나 제네릭 메서드는 문서화할 때 모든 타입 매개변수에 주석을 달아야 한다.** 다음 예를 보자.

```java
/**
 * An object that maps keys to values. A map cannot contain
 * duplicate keys; each key can map to at most one value.
 *
 * (Remainder omitted)
 *
 * @param <K> the type of keys maintained by this map
 * @param <V> the type of mapped values
 */
public interface Map<K, V> { ... }  
```

번역하면 다음과 같다.

```java
/**
 * 키와 값을 매핑하는 객체, 맵은 키를 중복해서 가질 수 없다.
 * 즉, 키 하나가 가리킬 수 있는 값은 최대 1개다.
 *
 * (나머지 설명은 생략)
 *
 * @param <K> 이 맵이 관리하는 키의 타입
 * @param <V> 매핑된 값의 타입
 */
public interface Map<K, V> { ... }  
```

**열거 타입의 경우에는 상수들에도 주석을 달아야 한다(public 메서드 포함)**. 다음 예를 보자.

```java
/**
 * An instrument section of a symphony orchestra.
 */
public enum OrchestraSection {
  /** Woodwinds, such as flute, clarinet, and oboe.     */
  WOODWIND,
  /** Brass instruments, such as french horn and trumpet. */
  BRASS,
  /** Percussion instruments, such as timpani and cymbals. */
  PERCUSSION,
  /** Stringed instruments, such as violin and cello. */
	STRING;
}
```

번역하면 다음과 같다.

```java
/**
 * 심포니 오케스트라의 악기 세션,
 */
public enum OrchestraSection {
  /** 플루트, 클라리넷, 오보 같은 목관악기. */
  WOODWIND,
  /** 프렌치 호른, 트럼펫 같은 금관악기. */
  BRASS,
  /** 탐파니, 심벌즈 같은 타악기. */
  PERCUSSION,
  /** 바이올린, 첼로 같은 현악기. */
  STRING;
}
```

**애너테이션 타입을 문서화할 때는 멤버들에도 모두 주석을 달아야 한다.** 다음 예를 보자.

```java
/**
 * Indicates that the annotated method is a test method that
* must throw the designated exception to pass.
*/
@Retention (RetentionPolicy.RUNTIME)
@Target (ElementType.METHOD)
public interface ExceptionTest {
  /**
   * The exception that the annotated test method must throw
   * in order to pass. (The test is permitted to throw any
   * subtype of the type described by this class object.)
  */
  Class<? extends Throwable> value();
}
```

번역하면 다음과 같다.

```java
/**
 * 이 애너테이션이 달린 메서드는 명시한 예외를 던져야만 성공하는 
 * 테스트 메서드임을 나타낸다.
 */
@Retention (RetentionPolicy. RUNTIME)
@Target(ElementType.METHOD)
public @interface ExceptionTest {
  /**
   * 이 애너테이션을 단 테스트 메서드가 성공하려면 던져야 하는 예외.
   * (이 클래스의 하위 타입 예외는 모두 허용된다.)
   */
  Class<? extends Throwable> value();
}  
```

패키지를 설명하는 문서화 주석은 package-info.java 파일에 작성한다. 자바 9부터 지원하는 모듈 관련 설명은 module-info.java 파일에 작성하면 된다.

스레드 안전성과 직렬화 가능성도 API 문서에 포함시키자. **클래스 혹은 정적 메서드가 스레드 안전하든 그렇지 않든, 스레드 안전 수준을 반드시 API 설명에 포함해야 한다**(아이템 82). 직렬화할 수 있는 클래스라면 직렬화 형태도 API 설명에 기술해야 한다(아이템 87).

또한 {@inheritDoc} 태그를 사용하면 상위 타입의 문서화 주석 일부를 상속할 수 있다. 사용하기에는 사용하기 까다로운 부분이 있으니 자세한 내용은 [오라클 공식 문서](http://bit.ly/2vqmCzj)를 참고하자.

자바 7에서 명령줄에서 -Xdoclint 스위치를 켜주면 프로그래머가 자바독 문서를 올바로 작성했는지 확인해준다(자바 8부터는 기본으로 작동하며, 체크스타일(checkstyle) 같은 IDE 플러그인을 사용하면 더 완벽하게 검사된다).

이번 아이템에서 다룬 기본적인 규칙 이상을 알아보려면 「문서화 주석 작성법」[Javadoc-guide]을 참고하자(저자는 이를 API 문서 작성법의 정석으로 여긴다).

마지막으로, 정말 잘 쓰인 문서인지를 확인하는 방법은 오직 **자바독 유틸리티가 생성한 웹페이지를 읽어보는 것 뿐이다.** 

## 마무리

- 문서화 주석은 API를 문서화하는 가장 훌륭하고 효과적인 방법이다.
- 문서화 주석를 작성할 때는 표준 규약을 일관되게 지키자.



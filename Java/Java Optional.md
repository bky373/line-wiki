## Optional 클래스
* 1.8 버전 부터 사용 가능하다.
* 기본적으로 Object 를 상속한다.
* null 값 또는 null 이 아닌 값을 포함하는 컨테이너 객체다.
* null이 아닌 값(value)이 있으면 isPresent()는 true를 반환하고, get()은 값을 반환한다.
* 추가 메서드로 orElse() 와 ifPresent()가 있다.  orElse() 메서드는 값이 없으면 기본값을 반환한다. ifPresent()는 값이 있으면 코드 블록을 실행한다.
* 가치 기반(value-based) 클래스이고 아래 주의할 점이 있다.
    * ID에 민감한(identity-sensitive) 작업의 경우, 예를 들어 참조 비교(==), 해시코드, 동기화 등이 사용되는 경우 예측불가한 결과가 발생할 수 있으니 피해야 한다.
  > [출처](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)

## orElse 와 orElseGet 비교
### orElse
* 메서드 선언
  ```java
  public T orElse(T other)
  ```
* 설명
  * 값이 있으면 값을 반환하고, 없으면 other를 반환한다.
  * other은 null 일 수 있으나, null로 반환하는 것은 Optional 사용 취지에 어긋난다.

### orElseGet
* 메서드 선언
  ```java
  public T orElseGet(Supplier<? extends T> other)
  ```
* 설명
  * 값이 있으면 값을 반환하고, 없으면 other를 호출하여 해당 호출의 결과를 반환한다.
  * 값이 존재하지 않고 other 이 null 일 경우 NullPointerException 을 던진다.


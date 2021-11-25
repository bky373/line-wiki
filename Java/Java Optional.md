## Optional 클래스

* Object를 상속한다.
* null 값 또는 null 이 아닌 값을 포함하는 컨테이너 객체다.
* null이 아닌 값(value)이 있으면 isPresent()는 true를 반환하고, get()은 값을 반환한다.
* 추가 메서드로 orElse() 와 ifPresent()가 있다.  orElse() 메서드는 값이 없으면 기본값을 반환한다. ifPresent()는 값이 있으면 코드 블록을 실행한다.
* 가치 기반(value-based) 클래스이고 아래 주의할 점이 있다.
    * ID에 민감한(identity-sensitive) 작업의 경우, 예를 들어 참조 비교(==), 해시코드, 동기화 등이 사용되는 경우 예측불가한 결과가 발생할 수 있으니 피해야 한다.
  > [출처](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)

# StringUtils.isBlank 와 StringUtils.isEmpty 차이

- StringUtils는 Apache Common 라이브러리에 선언된 클래스이다.

  <img width="310" alt="image" src="https://user-images.githubusercontent.com/49539592/142223219-d8a1805d-b433-436a-a106-affd3a04c742.png">

## StringUtils.isBlank
  - 선언
    ```java
    public static boolean isBlank(CharSequence cs)
    ```
  - 설명
    - Checks if a CharSequence is empty (""), null or whitespace only.
      > Whitespace 는 [`Character.isWhitespace(char)`](https://docs.oracle.com/javase/7/docs/api/java/lang/Character.html?is-external=true#isWhitespace-char-)를 참고하자.
  - 실행 및 결과
    ```java
    StringUtils.isBlank(null)      = true
    StringUtils.isBlank("")        = true
    StringUtils.isBlank(" ")       = true
    StringUtils.isBlank("bob")     = false
    StringUtils.isBlank("  bob  ") = false 
    ```
  - 결론
    - `""` 와 `null`, `whitespace` 만 있는 문자열 만 true
    - 나머지는 `false`

## StringUtils.isEmpty
  - 선언
    ```java
    public static boolean isEmpty(CharSequence cs)
    ```
  - 설명
    - Checks if a CharSequence is empty ("") or null.
  - 실행 및 결과

    ```java
    StringUtils.isEmpty(null)      = true
    StringUtils.isEmpty("")        = true
    StringUtils.isEmpty(" ")       = false
    StringUtils.isEmpty("bob")     = false
    StringUtils.isEmpty("  bob  ") = false
    ```
  - 결론
    - `""` 와 `null` 만 `true` 
    - 나머지는 `false` 

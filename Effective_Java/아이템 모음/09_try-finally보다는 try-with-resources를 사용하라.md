# 2장. 객체 생성과 파괴(p7~94)

## 아이템 9. try-finally보다는 try-with-resources를 사용하라(p47~50)

InputStream, OutputStream, java.sql.Connection 등을 사용할 때, 자원을 다 쓴 후에는 꼭 닫아주어야 한다. 전통적으로 try-finally 문 안에서 close 메서드를 호출해 직접 닫아주는 방법을 사용하였다.

```java
static String firstLineOfFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        br.close();
    }
}
```

하지만 try-finally 방식은 close를 까먹는다거나, 잡아야 할 예외를 놓친다거나, 자원을 늘릴 때 코드가 지저분해지는 단점을 가지고 있다.

**try-with-resources 의 등장은 이러한 문제를 모두 해결해주었다.** 다만 이 구조를 사용하는 경우 닫아야 하는 **자원이 AutoCloseable 인터페이스를 구현하도록 해야 한다**(AutoCloseable은 단순히 void를 반환하는 close 메서드 하나만을 정의한 인터페이스이다).

```java
static String firstLineOfFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(
        new FileReader(path))) {
        return br.readLine();
    }
}
```

> try-with-resources 버전이 짧고 읽기 수월할 뿐 아니라 문제를 진단하기도 훨씬 좋다. firstLineOfFile 메서드를 생각해보자. **readLine과 (코드에는 나타나지 않는) close 호출 양쪽에서 예외가 발생하면, close에서 발생한 예외는 숨겨지고 readLine에서 발생한 예외가 기록된다.** 이처럼 실전에서는 프로그래머에게 보여줄 예외 하나만 보존되고 여러 개의 다른 예외가 숨겨질 수도 있다. 숨겨진 예외들도 그냥 버려지지는 않고, 스택 추적 내역에 '숨겨졌다(suppressed)'는 꼬리표를 달고 출력된다.

try-with-resources에서도 catch 절을 사용할 수 있다. catch 절을 사용하여 try 문을 중첩하지 않고도 여러 개의 예외 처리를 할 수 있다.

```java
static String firstLineOfFile(String path, String defaultVal) {
        try (BufferedReader br = new BufferedReader(
                new FileReader(path))) {
            return br.readLine();
        // 파일을 열거나 데이터를 읽지 못했을 때 예외를 던지는 대신 기본값을 반환한다.
        } catch (IOException e) {
            return defaultVal;
        }
    }
```

> **꼭 회수해야 하는 자원을 다룰 때는 try-finally 말고, try-with-resources를 사용하자. 예외는 없다.**


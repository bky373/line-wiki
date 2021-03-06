# 12장. 직렬화(p449~482)

> 이번 장은 객체 직렬화를 다룬다. 객체 직렬화란 자바가 객체를 바이트 스트림으로 인코딩하고(직렬화) 그 바이트 스트림으로부터 다시 객체를 재구성하는(역직렬화) 메커니즘이다.
>
> 직렬화된 객체는 다른 VM에 전송하거나 디스크에 저장한 후 나중에 역직렬화할 수 있다. 이번 장은 직렬화가 품고 있는 위험과 그 위험을 최소화하는 방법에 집중한다.

## 아이템 88. readObject 메서드는 방어적으로 작성하라(p467~473)

readObject 메서드는 실질적으로 또 다른 public 생성자이기 때문에 다른 생성자와 똑같은 수준으로 주의를 기울여야 한다. 보통의 생성자처럼 readObject 메서드에서도 인수가 유효한지 검사해야 하고(아이템49) 필요하다면 매개변수를 방어적으로 복사해야 한다(아이템 50). 만약 유효성 검사에 실패하면 InvalidObjectException을 던지게 하여 잘못된 역직렬화가 일어나는 것을 막을 수 있다.

객체를 역직렬화할 때는 클라이언트가 소유해서는 안 되는 **객체 참조를 갖는 필드를 모두 반드시 방어적으로 복사해야 한다.**

final이 아닌 직렬화 가능 클래스라면 readObject와 생성자의 공통점이 하나 더 있다. 마치 생성자처럼 readObject 메서드도 재정의 가능 메서드를 (직접적으로든 간접적으로든) 호출해서는 안 된다(아이템 19). 

## 마무리

- readObject 메서드를 작성할 때는 언제나 public 생성자를 작성하는 자세로 임해야 한다. 
- 아래 안전한 readObject 메서드를 작성하는 지침을 요약해보았다.
  - private이어야 하는 객체 참조 필드는 각 필드가 가리키는 객체를 방어적으로 복사하라. 불변 클래스 내의 가변 요소가 여기 속한다.
  - 모든 불변식을 검사하여 어긋나는 게 발견되면 InvalidObjectException을 던진다. 방어적 복사 다음에는 반드시 불변식 검사가 뒤따라야 한다.
  - 역직렬화 후 객체 그래프 전체의 유효성을 검사해야 한다면 ObjectInputValidation 인터페이스를 사용하라(이 책에서는 다루지 않는다).
  - 직접적이든 간접적이든, 재정의할 수 있는 메서드는 호출하지 말자.



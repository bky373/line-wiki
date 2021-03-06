# 11장. 동시성(p413~448)

> 이번 장에는 동시성 프로그램을 명확하고 정확하게 만들고 문서화를 잘 하는 데 도움이 되는 조언들을 담았다.

## 아이템 81. wait와 notify보다는 동시성 유틸리티를 애용하라(p431~437)

지금은 wait와 notify를 사용해야 할 이유가 많이 줄어들었다. 자바 5에서 도입된 고수준의 동시성 유틸리티가 wait와 notify로 하드코딩해야 했던 일들을 대신 처리해주기 때문이다. **wait와 notifiy는 올바르게 사용하기가 아주 까다로우니 고수준 동시성 유틸리티를 사용하자.**

java.util.concurrent의 고수준 유틸리티는 세 범주로 나눌 수 있다. 실행자 프레임워크(아이템 80), 동시성 컬렉션(concurrent collection), 동기화 장치(synchronizer)다. 

동시성 컬렉션은 List, Queue, Map 같은 표준 컬렉션 인터페이스에 동시성을 가미해 구현한 고성능 컬렉션을 말한다. 동시성을 높이기 위해 동기화를 각자의 내부에서 수행한다(아이템 79). 따라서 **동시성 컬렉션에서 동시성을 무력화하는 건 불가능하며, 외부에서 락을 추가로 사용하면 오히려 속도가 느려진다.**

동시성 컬렉션은 동기화한 컬렉션을 낡은 유산으로 만들어버렸다. 대표적인 예로, **이제는 Collections.synchronizedMap보다는 ConcurrentHashMap을 사용하는 게 훨씬 좋다.** 

동기화 장치는 스레드가 다른 스레드를 기다릴 수 있게 하여, 서로 작업을 조율할 수 있게 해준다.

> 가장 자주 쓰이는 동기화 장치는 CountDownLatch와 Semaphore다. CyclicBarrier와 Exchanger는 그보다 덜 쓰인다. 그리고 가장 강력한 동기화 장치는 바로 Phaser다.

**시간 간격을 잴 때는 항상 System.currentTimeMillis가 아닌 System.nanoTime을 사용하자.** System.nanoTime은 더 정확하고 정밀하며 시스템의 실시간 시계의 시간 보정에 영향받지 않는다. 

wait 메서드는 스레드가 어떤 조건이 충족되기를 기다리게 할 때 사용한다. 락 객체의 wait 메서드는 반드시 그 객체를 잠근 동기화 영역 안에서 호출해야 한다. wait를 사용하는 표준 방식은 다음과 같다.

```java
synchronized (obj) {
    while (<조건이 충족되지 않았다>)
    	obj.wait(); // (락을 놓고, 깨어나면 다시 잡는다.)
    
    ... // 조건이 충족됐을 때의 동작을 수행한다.
}
```

**wait 메서드를 사용할 때는 반드시 대기 반복문(wait loop) 관용구를 사용하라, 반복문 밖에서는 절대로 호출하지 말자.** 

## 마무리

- wait와 notify를 직접 사용하는 것을 동시성 '어셈블리 언어'로 프로그래밍하는 것에 비유할 수 있다. 반면 java.util.concurrent는 고수준 언어에 비유할 수 있다. **새로운 코드를 작성한다면 wait와 notify를 사용할 필요가 없다.**
- 오직 레거시 코드를 유지보수해야 하는 경우에 wait는 표준 관용구에 따라 언제나 while 문 안에서 호출하자.
- 일반적으로 notify보다는 notifyAll을 사용해야 한다. 혹시라도 notify를 사용한다면 응답 불가 상태에 빠지지 않도록 각별히 주의하자.



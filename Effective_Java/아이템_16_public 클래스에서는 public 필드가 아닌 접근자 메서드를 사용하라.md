# 4장. 클래스와 인터페이스(p95~152)

## 아이템 16. public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라(p102~104)

public 클래스의 인스턴스 필드들은 외부에 직접 노출하지 않고 getter와 같은 접근자 메서드를 통해 접근하도록 해야 한다. 예를 들면 이런 식이다.

```java
class Point {
    private double x;
    private double y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public void setX(double x) {
        this.x = x;
    }

    public void setY(double y) {
        this.y = y;
    }
}
```

이 방식이 아니면 API가 공개되어 있어 클래스 내부 표현 방식을 마음대로 바꾸는 게 거의 불가능하다. 결국 public 클래스는 절대로 가변 필드를 직접 노출해서는 안 된다(행여 불변 필드라고 할지라도 노출을 피하는 것이 좋다).


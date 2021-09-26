# Java Chapter 17

### 1. 인터페이스의 기본과 그 의미

- 우리는 인스턴스에 상당히 많은 것을 담을 수 있습니다.
  - 즉 우리는 디자인된 클래스가 존재합니다. ( 어마어마한 기능이 존재 )
- 사용자 입장에서는 너무 클래스가 복잡해...
- 그래서 사용하기 좋게 이 클래스의 인터페이스를 생성합니다.
- 그리고 이 인터페이스를 사용자에게 제공
  - 사용자는 인터페이스를 가지고 해당 클래스 or 인스턴스를 활용할 수 있습니다.
  - 즉 인스턴스의 사용방법만 정의!
- 추상 메소드만 담고 있는 인터페이스
  - **메소드의 몸체를 갖지 않습니다.**
    - 즉 인스턴스를 생성할 수 없지만 참조변수 선언은 가능합니다.

```java
// 인터페이스는 클래스 정의에 의해서 구현이 되는 것!
interface Printable {
   public void print(String doc);   // == 추상 메소드
}

// implements Printable : Printer라는 클래스가 Printable의 인터페이스를 구현하겠다라는 의미!
// 즉 Printable안에 있는 추상 메소드를 내가 정의하겠다라는 의미
// implements에서 s가 붙은 이유 : 둘 이상의 인터페이스를 한 클래스에서 구현이 가능하기 때문에...
class Printer implements Printable {
   // 메소드의 호출 관계에서 메소드 오버라이딩 관계 성립 => @Override를 붙일 수 있씁니다.
   // 즉 print메소드를 정의하면서 오버라이딩 해버린 것 => 실제 몸체의 print메소드가 호출되는 것
   // (상속과 유사)
   public void print(String doc) {   
      System.out.println(doc);
   }
}

// 인터페이스형 참조변수 선언은 가능
// Printable prn의 참조변수는 Printable 인터페이스를 구현한 클래스의 인스턴스를 참조할 수 있습니다.
Printable prn = new Printer();
// 인터페이스 내의 추상 메소드만 호출 가능, Printer에 있는 퍼블릭 메소드는 호출 불가
prn.print("Hello");
```

- 상속과 구현
  - 상속은 하나의 클래스만 상속가능 했습니다.
  - 하지만 인터페이스는 상속과 별개로 1개 이상의 인터페이스를 구현할 수 있습니다.
  - Robot 인스턴스를 참조할 수 있는 참조변수 종류
    - `Machine a = new Robot();`
    - `Movable b = new Robot();`
    - `Runnable c = new Robot();`

```java
// Robot 클래스는 Machine 클래스를 상속한다.
// Robot 클래스는 Movable과 Runnable 인터페이스를 구현한다.
class Robot extends Machine implements Movable, Runnable {...}
```


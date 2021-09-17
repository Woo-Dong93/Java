# Java Chapter 15

### 1. 상속을 위한 두 클래스의 관계

- 상속의 특성
  - 하위 클래스는 상위 클래스의 모든 특성을 지닙니다.
  - 거기에 더하여 하위 클래스는 자신만의 추가적인 특성을 더하게 됩니다.
- 모바일폰 vs 스마트폰
  - 모바일폰을 스마트폰이 상속합니다.
  - `class 스마트폰 extends 모바일폰 {...}`
  - 즉 상속 관계에 있는 두 대상은 **IS-A** 관계를 가져야 합니다.
    - A is a B ( A는 일종의 B이다. ) => 이 관계일 때 상속의 후보가 되는 것!

- 상속과 IS-A 관계
  - 노트북은 컴퓨터다.
  - 전기자동차는 자동차다.
- IS-A 관계를 갖지 않는 두 클래스가 상속으로 연결되어 있다면 적절한 상속인지 의심해야 한다!



### 2. 메소드 오버라이딩

- 부모클래스와 자식클래스가 존재할 때
  - 자식클래스 인스턴스 참조변수로 자식클래스를 참조 가능
    - 자식과 부모 모든 기능 사용 가능
  - 부모클래스인스터스 참조변수로 자식클래스를 참조 가능
    - 자기 부모클래스의 기능만 사용 가능

- 상위 클래스의 참조변수가 참조할 수 있는 대상의 범위

```java
// 상속
class SmartPhone extends MobilePhone {....}

// 참조 방식
SmartPhone phone = new SmartPhone("010-555-777", "Nougat");
// 이 역은 성립하지 않습니다 / 또한 기능이 모바일 폰으로 제한
MobilePhone phone = new SmartPhone("010-555-777", "Nougat");
```

- 참조변수의 참조 가능성에 대한 정리
  - `StrawberryCheeseCake`를 참조할 수 있는 참조변수의 종류는 총 3가지 가능
  - `Cake` / `CheeseCake` / `StrawberryCheeseCake`

```java
class Cake {
   public void sweet() {...}
}

class CheeseCake extends Cake {
   public void milky() {...}
}

class StrawberryCheeseCake extends CheeseCake {
   public void sour() {....}
}

// 가능
Cake cake1 = new StrawberryCheeseCake();
CheeseCake cake2 = new StrawberryCheeseCake();
```

- 예시 ( 기능 제한 )

![](../img/25.png)

- 참조변수 간 대입과 형 변환
  - 컴파일러가 코드를 해석할 때 상속 관계를 보고 참조가 가능하면 참조 변수에 대상을 넣어줍니다.

```java
class Cake {
   public void sweet() {...}
}

class CheeseCake extends Cake {
   public void milky() {...}
}

CheeseCake ca1 = new CheeseCake();
Cake ca2 = ca1;    // 가능!

Cake ca3 = new CheeseCake();
// 컴파일러는 문장단위로 기억 ( 이전것들 기억하지 않음 ) => ca3가 Cake형 참조변수 사실만 앎
// 컴파일러 및 가상머신은 ca3가 참조하는 대상이 cake인스턴스로 판단 => 허용하지 않음
// 허용을 안한 이유는 자바 문법을 불필요하게 만들기 싫어서...
// 해결법 : 강제 형변환
CheeseCake ca4 = ca3;     // 불가능!
```

- 참조 변수의 참조 가능성 ( 배열 기반 )

```java
class Cake {
   public void sweet() {...}
}

class CheeseCake extends Cake {
   public void milky() {...}
}

// 배열까지 모두 가능 ( 상속의 영향력이 배열에도 적용 )
Cake cake = new CheeseCake();
CheeseCake[] cakes = new CheeseCake[10];
Cake[] cakes = new CheeseCake[10];
```

- 메소드 오버라이딩 ( 1 )

```java
class Cake {
   public void yummy() {
      System.out.println("Yummy Cake");
   }
}

// Cake의 yummy()를 오버라이딩
// 반환형, 메소드 이름, 매개 변수가 모두 동일 == 오버라이딩
class CheeseCake extends Cake {
   public void yummy() { 
      System.out.println("Yummy Cheese Cake");
   }
}

public static void main(String[] args) {
   Cake c1 = new CheeseCake();
   CheeseCake c2 = new CheeseCake();
   
   // 가려버렸기 때문에 부모클래스로 참조하고 있어도 오버라이딩 된 메소드가 호출
   c1.yummy(); // Yummy Cheese Cake
   c2.yummy(); // Yummy Cheese Cake
   
   // 이것은 아지가 메소드 호출 가능
   Cake c3 = new Cake();
}
```

- 메소드 오버라이딩 ( 2 )

```java
class Cake {
   public void yummy() {...}
}
class CheeseCake extends Cake {
   public void yummy() {...}    // Cake의 yummy를 오버라이딩
}
Class StrawberryCheeseCake extends CheeseCake {
   public void yummy() {...}    // 그리고 CheeseCake의 yummy를 오버라이딩
}

public static void main(String[] args) {
   Cake c1 = new StrawberryCheeseCake();
   CheeseCake c2 = new StrawberryCheeseCake();
   StrawberryCheeseCake c3 = new StrawberryCheeseCake();
   // 어떠한 참조볏수로 접근을 해도 안가려진 StrawberryCheeseCake의 메소드만 호출됩니다.
   c1.yummy();   
   c2.yummy();
}
```

- 오버라이딩 된 메소드 호출하는 방법
  - **인스턴스 외부에서는 호출하는 방법이 없습니다.**
  - 하지만 인스턴스 내부에서는 `super`를 이용해 호출 가능

```java
class Cake {
   public void yummy() {
      System.out.println("Yummy Cake");
   }
}

class CheeseCake extends Cake {
   public void yummy() {
      super.yummy();      
      System.out.println("Yummy Cheese Cake");
   }
	
   // 다른 메소드에서도 얼마든지 호출 가능 
   public void tasty() {
      super.yummy();    
      System.out.println("Yummy Tasty Cake");
   }
}
```

- 인스턴스 변수와 클래스 변수도 오버라이딩이 되는가?
  - **인스턴수 변수는 오버라이딩 되지 않습니다.**
  - 참조변수의 형에 따라 접근하는 멤버가 결정됩니다.

```java
class Cake {
   public int size;
   ....
}

class CheeseCake extends Cake {
   public int size;
   ....
}

CheeseCake c1 = new CheeseCake();
c1.size = ... // CheeseCake의 size에 접근

Cake c2 = new CheeseCake();
C2.size = ... // Cake의 size에 접근
```

- `instance of` 연산자
  - 왼쪽에는 참조변수
  - 오른쪽에는 클래스 이름
  - `true` or `false`를 반환
  - 이 참조변수가 참조하는 인스턴스를 클래스 이름의 참조형 변수로 참조가 가능합니까?

```java
class Cake {
}

class CheeseCake extends Cake {
}

class StrawberryCheeseCake extends CheeseCake{
}

public static void main(String[] args) {
   // StrawberryCheeseCake의 클래스는 Cake / CheeseCake / StrawberryCheeseCake의 인스턴스로 참조 가능
   Cake cake = new StrawberryCheeseCake();
   // 모두 true
   if(cake instanceof Cake) {...}
   
   if(cake instanceof CheeseCake) {...}

   if(cake instanceof StrawberryCheeseCake) {...}
}
```

- `instance of` 연산자의 활용
  - **상속 : 연관된 일련의 클래스들에 대해 공통적인 규약을 정의할 수 있습니다.**

```java
// 3개의 클래스 상속 관계
class Box {
   public void simpleWrap() {
      System.out.println("Simple Wrapping");
   }
}

class PaperBox extends Box {
   public void paperWrap() {
      System.out.println("Paper Wrapping");
   }
}

class GoldPaperBox extends PaperBox {
   public void goldWrap() {
      System.out.println("Gold Wrapping");
   }
}

public static void main(String[] args) {
   Box box1 = new Box();
   PaperBox box2 = new PaperBox();
   GoldPaperBox box3 = new GoldPaperBox();
	
   // 각자 자기 메소드 호출 
   wrapBox(box1);
   // Box2의 참조변수를 box의 참조변수에 저장 가능
   // Box box = new PaperBox();
   wrapBox(box2);
   wrapBox(box3);
}

// 이 방법보다는... 상속의 특징을 이용해 개선한다면?
public static void wrapBox(Box box) {
   if (box instanceof GoldPaperBox) {
      ((GoldPaperBox)box).goldWrap();
   }
   else if (box instanceof PaperBox) {
      ((PaperBox)box).paperWrap();
   }
   else {
      box.simpleWrap();
   }
}
```

- 개선 ( 오버라이딩 )
  - 결과 동일
  - 상속 특성 이용

```java
class Box {
   public void Wrap() {
      System.out.println("Simple Wrapping");
   }
}

class PaperBox extends Box {
   public void Wrap() {
      System.out.println("Paper Wrapping");
   }
}

class GoldPaperBox extends PaperBox {
   public void Wrap() {
      System.out.println("Gold Wrapping");
   }
}

public static void wrapBox(Box box) {
   box.Wrap();
}
```


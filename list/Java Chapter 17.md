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



### 1-1 인터페이스의 본질적 의미

- 프린터는 종류가 수십가지 ( 삼성, LG 등... )
  - 각자 회사별로 디바이스 드라이버를 제공합니다.
- MS Window의 운영체제와 프린터는 연결
- MS는 프린터 별로 사용하는 방법이 다르면 매우 어려운 문제가 발생...
- MS Window는 그럼 내가 **인터페이스**를 만들어 줄게! 이것을 바탕으로 드라이버 개발해!
  - 그 클래스가 무엇이든 간에 사용방법은 동일하게 됩니다.
- 그러면 각 회사들은 인터페이스를 구현한 클래스를 개발!
  - MS는 어떤 회사의 프린터 상관없이 인터페이스를 바탕으로 동일하게 드라이버를 사용 가능
  - 이것이 인터페이스의 장점!

![](../img/26.png)

```JAVA
public static void main(String[] args) {
   String myDoc = "This is a report about...";
   
   // 삼성 프린터로 출력
   Printable prn = new SPrinterDriver();
   prn.print(myDoc);
   System.out.println();

   // LG 프린터로 출력
   prn = new LPrinterDriver();
   prn.print(myDoc);
}
```



### 2. 인터페이스의 문법 구성과 추상 클래스

- 인터페이스에 선언되는 메소드와 변수

```JAVA
interface Printable {
   // public 선언을 하지 않더라도 기본으로 선언된 것으로 약속되어 있음 => 그래야 외부에서 사용가능
   // default 선언이 되지 않는 것이 중요! 
   public void print(String doc);    // 추상 메소드
}

// 인스턴스 변수는 선언 불가능하지만 상수는 선언이 가능합니다.
interface Printable {
   //public static final을 생략하면 자동으로 붙게 됩니다 => 선언할 수 없으니...
   public static final int PAPER_WIDTH = 70;
   public static final int PAPER_HEIGHT = 120;
   public void print(String doc);
}
```

- 인터페이스간 상속( 문제 상황의 제시 )
  - 갑자기 기존 프린터에 칼라 프린터 기능을 추가!
  - 그래서 MS가 새로운 인터페이스 추상 메소드를 추가
    - 이렇게 되면... 기업 입장에서는 인터페이스 안에 있는 추상 메소드를 모두 구현해야 함
    - 흑백 프린터만 지원하는 클래스인데.. 왜 이 클래스를 수정해야 할까?? => 문제

![](../img/27.png)

- 이 문제를 해결하기 위해 나온 문법 => **인터페이스 상속**

```JAVA
interface Printable {
   void print(String doc);
}

// 새로운 인터페이스를 생성, 그리고 기존 인터페이스를 상속 ( extends 키워드 사용 )
// 추가로 메소드를 하나 더 생성
interface ColorPrintable extends Printable {
   void printCMYK(String doc);
}

// 기존의 프린터 드라이버는 수정할 필요가 없습니다.
class SPrinterDriver implements Printable {
   ...
}   

// 컬러 프린터 드라이버는 ColorPrintable 인터페이스를 구현하면 됩니다.
class Prn909Drv implements ColorPrintable {
   @Override
   public void print(String doc) {   // 흑백 출력
      System.out.println("black & white ver");
      System.out.println(doc);
   }
   
   @Override
   public void printCMYK(String doc) {   // 컬러 출력
      System.out.println("CMYK ver");
      System.out.println(doc);
   }
}
```

- 인터페이스의 디폴트 메소드
  - 거의 사용하지 않는 문법!!

![](../img/28.png)

- 총 256개의 인터페이스가 존재하는 상황에서 모든 인터페이스에 다음 추상 메소드를 추가해야 한다면?
  - `void printCMYK(String doc);`
  - 인터페이스간 상속?
-  물론 인터페이스간 상속으로 문제 해결 가능하합니다... 다만 인터페이스의 수가 256개 늘어날 뿐!

- 이것의 해결책이 **인터페이스의 디폴트 메소드**

```java
// 다음 디폴트 메소드로 이 문제를 해결하면 인터페이스의 수가 늘어나지 않는다. 
// 이 인터페이스 메소드는 구현해도 그만... 안해도 그만!
default void printCMYK(String doc) { }    // 디폴트 메소드
```

- 디폴트 메소드의 효과

```java
interface Printable {
   void print(String doc);
}

// 아래의 인터페이스로 교체 ( 새로운 기능 추가 )
interface Printable {
   void print(String doc);
   default void printCMYK(String doc) { }
}

// 기존 드라이버는 수정안해도 됩니다!
class SPrinterDriver implements Printable {
   @Override
   public void print(String doc) {...}
}

// 필요한 드라이버만 구현!!
class Prn909Drv implements Printable {
   @Override
   public void print(String doc) {...}
   
   @Override
   public void printCMYK(String doc) {...}
}

```

- 인터페이스의 static 메소드
  - 인터페이스에서도 **static 메소드**를 정의할 수있습니다.
  - 호출 방법은 클래스의 static 메소드 호출 방법과 동일합니다.

```java
interface Printable { 
   static void printLine(String str) {
      System.out.println(str);
   }

   default void print(String doc) {
      printLine(doc); // 인터페이스의 static 메소드 호출 가능
   }
}
```

- 인터페이스 대상의 `instanceof` 연산
  - `if( 참조변수 instanceof Cake)` 
    - 참조변수가 참조하는 인스턴스가 Cake 인스턴스 이거나 Cake를 부모클래스로 상속하는 클래스이면 `true`를 반환
    - 참조변수가 참조하는 인스턴스를 Cake로 형변환할 수 있어?
  - `if( 참조변수 instanceof Cake)`  : 인터페이스 적용
    - 참조변수가 참조하는 인스턴스가 **Cake를 직접(간접) 구현한 클래스의 인스턴스이면 true 반환**
      - 직접 구현 : `Class A implements Cake;`
      - 간접 구현 : `Class B extends A;`

- 인터페이스 대상 `instanceof` 연산의 예

```java
interface Printable {
   void printLine(String str);
}

// 직접구현
class SimplePrinter implements Printable { 
   public void printLine(String str) {
      System.out.println(str);
   }
}

// 간접구현
class MultiPrinter extends SimplePrinter {
   public void printLine(String str) {
      super.printLine("start of multi...");
      super.printLine(str);
      super.printLine("end of multi");
   }
}

public static void main(String[] args) {
   Printable prn1 = new SimplePrinter();
   Printable prn2 = new MultiPrinter();
	
   // true 
   if(prn1 instanceof Printable)
      prn1.printLine("This is a simple printer.");
   System.out.println();
    
   // true 
   if(prn2 instanceof Printable)
      prn2.printLine("This is a multiful printer.");
}
```

- 인터페이스의 또 다른 용도 ( **Maker 인터페이스** )
  - 무언가를 표시하는 목적으로 사용하는 인터페이스
  - 클래스에 특정 표시를 해두기 위한 목적으로 정의한 인터페이스
    - 구현할 메소드가 없는 것이 흔한 일입니다!

```java
interface Upper { }   // 마커 인터페이스
interface Lower { }   // 마커 인터페이스

interface Printable {
   String getContents();
}

// Upper 인터페이스는 구현할 메소드가 없음 => 비어있기 때문에... ( 단순 표시? )
class Report implements Printable, Upper {
   // 레포트 내용 담는 변수 
   String cons;

   Report(String cons) {
      this.cons = cons;
   }
   // 레포트 정보를 반환 
   public String getContents() {
      return cons;
   }
}

// 참조변수가 Printable => Printable 인터페이스를 직접 or 간접 구현한 클래스의 인스턴스만 인자로 전달해라 => Report 인스턴스는 전달 가능!
public void printContents(Printable doc) {
   
   if(doc instanceof Upper) { // true => 구현하고 있으니 
      // 대문자로 모두 변경 
      System.out.println((doc.getContents()).toUpperCase());
   }
   else if(doc instanceof Lower) {
      // 소문자로 모두 변경 
      System.out.println((doc.getContents()).toLowerCase());
   }
   else
      System.out.println(doc.getContents());
}
```

- 추상 클래스
  - 하나 이상의 추상 메소드를 지니는 클래스를 가리켜 **추상 클래스**라고 합니다.
  - 하지만 **추상 클래스를 대상으로는 인스턴스 생성이 불가능합니다.**
    - 물론 참조변수 선언은 가능!
    - 인스턴스 변수, 인스턴스 메소드 모두 구현 가능!
  - 나는 이것을 인스턴스화 되는 것을 원하지 않는다 => 이것을 상위 클래스로 디자인 했고 이것을 상속하는 하위클래스가 이 추상 메소드의 모양을 채워서 사용했으면 좋겠어! => 추상 클래스

```java
// 하나 이상의 추상 메소드를 가지고 있으므로 abstract 키워드를 사용해 추상클래스로 명시해야 합니다.
public abstract class House {    // 추상 클래스
   public void methodOne() {
      System.out.println("method one");
   }
   
   // 인터페이스와 달리 abstract 키워드 사용
   // 내가 클래스를 정의하고 있지만 나는 의도적으로 메소드의 몸체를 구현하지 않았다라는 의미 
   public abstract void methodTwo();    // 추상 메소드
}
```


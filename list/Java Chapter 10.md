# Java Chapter 10

### 1. static 선언을 붙여서 선언하는 클래스 변수

- 특정 변수를 공유하고 싶을때 `static`을 사용
- 인스턴스 생성과 별개로 하나만 존재하고 어디서나 접근 가능
  - 인스턴스 변수 : 인스턴스가 생성될 때 생성
  - 클래스 변수 : 클래스 내부에 위치하지만 하나만 존재
    - 인스턴스를 생성하지 않아도 특정 메모리 공간에 별도로 저장

```java
class InstCnt {
   // 현재는 default 선언, private로 선언하면 클래스 내부에서만 접근 가능
   // 자리를 빌려서 쓰다보니 이 클래스의 접근 지시자 규칙을 따라야 합니다.
   static int instNum = 0;   // 클래스 변수 (static 변수)
   
   InstCnt() {
      instNum++;  
      System.out.println("인스턴스 생성: " + instNum);
   }
} 
class ClassVar {
   public static void main(String[] args) {
      InstCnt cnt1 = new InstCnt(); // 1
      InstCnt cnt2 = new InstCnt(); // 2
      InstCnt cnt3 = new InstCnt(); // 3
   }
}
```

- 클래스 내부 접근
  - static 변수가 선언된 클래스 내에서는 이름만으로 직접 접근 가능
- 클래스 외부접근
  - private으로 선언되지 않으면 클래스 외부에서 접근 가능 => public
  - **클래스 또는 인스턴스의 이름을 통해 접근**

```java
class AccessWay {
   // default
   static int num = 0;

   AccessWay() { incrCnt(); }
   void incrCnt() {
      num++;   // 클래스 내부에서 이름을 통한 접근
   }
}

class ClassVarAccess {
   public static void main(String[] args) {
      AccessWay way = new AccessWay();
      way.num++;    // 외부에서 인스턴스의 이름을 통한 접근
      AccessWay.num++;    // 외부에서 클래스의 이름을 통한 접근 => 더 선호
      System.out.println("num = " + AccessWay.num); // 3
   }
}
```

- 클래스 변수의 초기화 시점과 초기화 방법
  - 생성자 안에서 초기화하면 안됩니다 => 계속 초기화 발생
  - JVM => Main 메소드 호출 => 필요한 정보만 읽어드림 ( 유연한 구조 )
    - 활용한 클래스만 읽어드림
    - `InstCnt.instNum`을 보고 InstCnt 클래스를 읽음 => static 변수를 캐치 및 메모리 공간에 저장
    - 즉 초기화 시점 : JVM이 읽는 순간

```java
class InstCnt {
   // 올바른 초기화 
   static int instNum = 100;

   InstCnt() {
      instNum++;
      System.out.println("인스턴스 생성: " + instNum);
   }
}

class OnlyClassNoInstance {
   public static void main(String[] args) {
      InstCnt.instNum -= 15;   // 인스턴스 생성 없이 instNum에 접근
      System.out.println(InstCnt.instNum);
   }
}
```

- 클래스 변수의 활용
  - 인스턴스 별로 가지고 있을 필요가 없는 변수
    - 값의 참조가 목적인 변수
    - 값의 공유가 목적인 변수
  - 그 값이 외부에서 참조하는 값이라면 `public`로 선언



### 2. static 선언을 붙여서 정의하는 클래스 메소드

- 클래스 변수와 성격이 동일
- 다른 메모리 공간에 저장

```java
class NumberPrinter {
   private int myNum = 0;
   static void showInt(int n) { System.out.println(n); }
   static void showDouble(double n) {System.out.println(n); }
   
   void setMyNumber(int n) { myNum = n; }
   // 내부접근
   void showMyNumber() { showInt(myNum); }
}

class ClassMethod {
   public static void main(String[] args) {
      // 외부접근 => 더 선호
      NumberPrinter.showInt(20);      
      NumberPrinter np = new NumberPrinter();
      // 외부접근
      np.showDouble(3.15);
      np.setMyNumber(75);
      np.showMyNumber();
   }
}
```

- 클래스 메소드로 정의하는 것이 옳은 경우
  - 단순 기능 제공이 목적인 메소드
  - 인스턴스 변수와 관련 지을 이유가 없는 메소드
- 클래스 메소드에서 인스턴스 변수에 접근이 가능할까?
  - 불가능 => 다른 공간에 위치하고 있기 때문에...

```java
class AAA {
   int num = 0;   
   static void addNum(int n) {
      num += n;    
   }
}
```



### 3. System.out.println 그리고 public static void main()

- `System.out.println`
  - System : 클래스
  - out : 클래스 변수, 참조변수 ( 다시 점을 찍고 접근하니 )
  - println : 인스턴스의 메소드
- `public statc void main()`
  - main 메소드는 하나만 존재 => static
  - public, void : 자바와 우리의 약속 => 우리가 main을 정의하는데 반환하지말고 public로 선언해줘!
    - 외부 (이클립스 등) 툴에서 main 메소드를 실행 => 요청 자체가 클래스 외부에서 이루어지기 때문에...

```java
// 우리가 넣지 않아도 컴파일러가 import java.lang.*;를 넣어줍니다.
// 그리고 java.lang 생략 가능
// 만약에 java.lang에 들어있는 클래스와 동일한 이름으로 생성시 에러 발생 => 조심
java.lang.System.out.println(...);
```

- main 메소드를 어디에 위치시킬 것인가?
  - 위치 노상관 => static이기 때문...

```java
class Car {
   void myCar() {
      System.out.println("This is my car");
   }

   public static void main(String[] args) {
      Car c = new Car();
      c.myCar();
      Boat t = new Boat();
      t.myBoat();
   }
}

class Boat {
   void myBoat() {
      System.out.println("This is my boat");
   }
}
```



### 4. 또 다른 용도의 static 선언

- static 초기화 블록
  - 선언과 동시에 할당하면 된다.
  - 하지만 그 값이 변동성이 있는 것이라면?

```java
class DateOfExecution {
   static String date;    // 프로그램의 실행 날짜를 저장하기 위한 변수
    
   // static 블록 => 메모리 공간에 할당이 된 직후 실행
   static {
     LocalDate nDate = LocalDate.now();
     date = nDate.toString();
   } 
   
   public static void main(String[] args) {
      System.out.println(date);
   }
}
```

- static import 선언

```java
// java.lang.Math.PI
System.out.println(Math.PI);

// import static java.lang.Math.PI => 임포트로 static을 활용해 선언하면 그냥 PI로 사용 가능
// 하지만 PI의 코드 출저가 코드레벨에서 안보이니 안좋은 문법
System.out.println(PI);
```


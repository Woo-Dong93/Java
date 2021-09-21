# Java Chapter 16

### 1. 상속이 도움이 되는 상황의 소개

- 상속이란?
  - A (부모) ← B ( 자식 )
  - 자식 클래스의 인스턴스를 생성해서 부모 클래스의 참조변수로 참조할 수 있어!
  - 메소드 오버라이딩 : A의 메소드와 B의 메소드가 동일(반환값, 이름, 인자)하면 자식클래스가 부모클래스의 메소드를 오버라이딩!
- 단순한 인맥 관리 프로그램 ( 관리 대상은 둘 )
  - 프로그램을 개발할 때에는 안정성( 모든 요구사항 충족 ) + 확장성

```JAVA
class UnivFriend {     // 대학 동창
   private String name;
   private String major;    // 전공
   private String phone;

   public UnivFriend (String na, String ma, String ph) {
      name = na;
      major = ma;
      phone = ph;
   }
   public void showInfo() {
      System.out.println("이름: " + name);
      System.out.println("전공: " + major);
      System.out.println("전화: " + phone);
   }
}
```

```JAVA
class CompFriend {     // 직장 동료
   private String name;
   private String department;     // 부서
   private String phone;

   public CompFriend (String na, String de, String ph) {
      name = na;
      department = de;
      phone = ph;
   }
   public void showInfo() {
      System.out.println("이름: " + name);
      System.out.println("부서: " + department);
      System.out.println("전화: " + phone);
   }
}
```

- 두 클래스를 대상을 하는 코드
  - 대상이 늘어나게 된다면 늘어난 수 만큼 코드도 복잡해지며 클래스도 추가로 필요

```JAVA
public static void main(String[] args) {   
   UnivFriend[] ufrns = new UnivFriend[5];
   int ucnt = 0;

   CompFriend[] cfrns = new CompFriend[5];
   int ccnt = 0;

   ufrns[ucnt++] = new UnivFriend("LEE", "Computer", "010-333-555");
   ufrns[ucnt++] = new UnivFriend("SEO", "Electronics", "010-222-444");

   cfrns[ccnt++] = new CompFriend("YOON", "R&D 1", "02-123-999");
   cfrns[ccnt++] = new CompFriend("PARK", "R&D 2", "02-321-777");

   for(int i = 0; i < ucnt; i++) {
      ufrns[i].showInfo();
      System.out.println();
   }

   for(int i = 0; i < ccnt; i++) {
      cfrns[i].showInfo();
      System.out.println();
   }
}
```

- 상속 기반의 문제 해결
  - 코드의 유연한 구조와 확장성을 위해 상속 사용

```JAVA
// 공통적인 속성을 부모클래스로 정의
class Friend {
   protected String name;
   protected String phone;
   
   public Friend(String na, String ph) {
      name = na;
      phone = ph;
   }
   public void showInfo() {
      System.out.println("이름: " + name);
      System.out.println("전화: " + phone);
   }
}

class CompFriend extends Friend {
   private String department;

   public CompFriend(String na, String de, String ph) {
      super(na, ph);
      department = de;
   }
   public void showInfo() {
      super.showInfo();
      System.out.println("부서: " + department);
   }
}

class UnivFriend extends Friend {
   private String major;

   public UnivFriend(String na, String ma, String ph) {
      super(na, ph);
      major = ma;
   }
   public void showInfo() {
      super.showInfo();
      System.out.println("전공: " + major);
   }
}
```

- 상속으로 묶은 결과

```JAVA
public static void main(String[] args) {
   Friend[] frns = new Friend[10];
   int cnt = 0;
   
   frns[cnt++] = new UnivFriend("LEE", "Computer", "010-333-555");
   frns[cnt++] = new UnivFriend("SEO", "Electronics", "010-222-444");
   frns[cnt++] = new CompFriend("YOON", "R&D 1", "02-123-999");
   frns[cnt++] = new CompFriend("PARK", "R&D 2", "02-321-777");
   
   // 모든 동창 및 동료의 정보 전체 출력
   for(int i = 0; i < cnt; i++) {
      frns[i].showInfo();      // 오버라이딩 한 메소드가 호출된다.
      System.out.println();
   }
}
```



### 2. Object 클래스와 final 선언 그리고 @Override

- 모든 클래스는 Object 클래스를 상속합니다.
  - 상속하는 클래스가 없으면 컴파일러가 Object 클래스를 상속하도록 구성함
    - java.lang.Obejct
  - 만약 다른 클래스를 `extends`키워드를 사용해 상속한다고 해도 상속한 클래스는 결국 Object를 상속
- 그럼 왜?
  - JVM은 생성된 인스턴스들을 모두 컨트롤 해야합니다.
  - 그래서 이것들을 각각 관리하기 위해는 어려움이 있습니다.
  - 그래서 그것을 일련의 규칙으로 관리하기 위해서 Object 참조형 변수만 있으면 모두 관리할 수 있게 됩니다.
- 모든 클래스가 Object를 직접 또는 간접상속하므로

```java
   // System.out.println
   // 모든 인스턴스의 부모이기 때문에 모두 인자로 받을 수 있습니다.
   public void println(Object x) {
      . . .
      // toString 메소드는 Object 클래스의 메소드였음을 알 수 있습니다.    
      String s = x.toString();
      . . .
   }
```

- 프로그래머가 정의하는 toString은 메소드 오버라이딩

```java
class Cake {
   // Object 클래스의 toString 메소드를 오버라이딩
   public String toString() {
      return "My birthday cake";
   }
}

class CheeseCake extends Cake {
   // Cake 클래스의 toString 메소드를 오버라이딩
   public String toString() {
      return "My birthday cheese cake";
   }
}
```

- 클래스와 메소드의 final 선언

```java
public final class MyLastCLS {...}
   // MyLastCLS 클래스는 다른 클래스가 상속할 수 없음
       
class Simple {
   // 아래의 메소드는 다른 클래스에서 오버라이딩 할 수 없음
   public final void func(int n) {...}
}
```

- @Override
  - 자바 컴파일러에게 알려주는 것
  - 우리가 실수로 오버라이딩을 하고자 했는데 오버로딩이 되는 상황을 방지할 수 있다.
  - 즉 안전성을 높이기 위한 문법

```java
class ParentAdder {
   public int add(int a, int b) {
      return a + b;
   }
}

class ChildAdder extends ParentAdder {
   // 없어도 컴파일은 되지만 작성시 안정성을 부여 
   // 매개면수가 다르기 때문에 오버라이딩을 할 수 없습니다 => 컴파일러가 에러를 발생 
   @Override
   public double add(double a, double b) {
      System.out.println("덧셈을 진행합니다.");
      return a + b;
   }
}
```


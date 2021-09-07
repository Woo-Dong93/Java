# Java Chapter 11

### 1. 메소드 오버로딩

- 호출된 메소드를 찾을 때 참조하게 되는 두 가지 정보
  - 메소드의 이름
  - 메소드의 매개변수 정보
- 따라서 이 둘 중 하나의 형태가 다른 메소드를 정의하는 것이 가능합니다.
  - 매개변수의 수가 달라도 가능
  - 매개변수의 형이 달라도 가능
  - **하지만 반환형은 오버로딩 조건이 아닙니다!**

```java
// 메소드 오버로딩
class MyHome {
   void mySimpleRoom(int n) {...}
   void mySimpleRoom(int n1, int n2) {...}
   void mySimpleRoom(double d1, double d2) {...}
}
```

- 오버로딩 관련 피해야할 애매한 상황

```java
class AAA {
   void simple(int p1, int p2) {...} // 1
   void simple(int p1, double p2) {...} // 2
}

// 어떤 메소드가 호출?
// 캐릭터형은 int형으로 자동 형변환 => 2번이 없으면 1번 메소드가 호출
// 캐릭터형은 double형으로 자동 형변환 => 1번이 없으면 2번 메소드가 호출
// 뭔가 모호한 상황 => 그래서 자동 형변환이 일어나지 않도록 구현해야 합니다! ( 강제 형변환 사용 )
AAA inst = new AAA();
inst. simple(7, 'K');      
```

- 생성자의 오버로딩

```java
class Person {
   private int regiNum;    // 주민등록 번호
   private int passNum;    // 여권 번호
   
   Person(int rnum, int pnum) {
      regiNum = rnum;
      passNum = pnum;
   }

   Person(int rnum) {
      regiNum = rnum;
      passNum = 0;
   }
   
   void showPersonalInfo() {...}
}
```

- 키워드 `this`를 이용한 다른 생성자의 호출
  - `this` : 이 인스턴스를 의미
  - 생성자 안에서 쓰이면 오버로딩된 다른 생성자를 호출

```java
Person(int rnum) {
   // 이 인스턴스의 오버로딩된 다른 생성자를 호출
   this(rnum, 0);
}
```

- 키워스 `this`를 이용한 인스턴스 변수의 접근

```java
class SimpleBox {
   private int data;

   SimpleBox(int data) {
      this.data = data;
   }
}
```



### 2. String 클래스

- String 인스턴스 생성의 두 가지 방법

```java
String str1 = new String("Simple String");
// 실제로 인스턴스 생성 => 참조 주소 반환 => str2는 이 인스턴스를 참조
String str2 = "The Best String";
```

- String 인스턴스와 println 메소드
  - 다양한 인자를 받을 수 있도록 오버로딩 되어 있습니다.
  - 자바는 문자열을 사용하면 String 인스턴스를 생성

```java
void println() {...} // 개행 => 인자를 안받으면...
void println(int x) {...}
void println(String x) {...}
```

- 문자열 생성 방법 두가지 차이점

```java
class ImmutableString {
   public static void main(String[] args) {
      // 이 2개는 동일
      String str1 = "Simple String";
      String str2 = "Simple String";
   		
      // 이 2개는 다름 
      String str3 = new String("Simple String");
      String str4 = new String("Simple String");
   
      if(str1 == str2)
         System.out.println("str1과 str2는 동일 인스턴스 참조"); // true
      else
         System.out.println("str1과 str2는 다른 인스턴스 참조");
   
      if(str3 == str4)
         System.out.println("str3과 str4는 동일 인스턴스 참조");
      else
         System.out.println("str3과 str4는 다른 인스턴스 참조"); // true
   }
}
```

- String 인스턴스는 Immutable 인스턴스
  - Immutable 인스턴스 : 생성과 동시에 데이터가 결정 => 한번 결정되면 데이터 변경 불가능
    - 제거하고 새로 만들어야 함
  - 생성되는 인스턴스의 수를 **최소화** => 효율성 고성
  - 즉 이미 생성된 똑같은 문자열의 인스턴스가 존재 => 그 주소값을 던져버림
    - 값을 바꿀수 없는 Immutable 인스턴스이기 때문에 문제가 발생하지 않음 => 참조가 끝!

```java
public static void main(String[] args) {
   String str1 = "Simple String";
   String str2 = str1; 
   . . . 
```

- String 인스턴스 기반 switch문 구성
  - 자바 7버전부터 추가

```java
public static void main(String[] args) {
   String str = "two";
   
   switch(str) {
   case "one":
      System.out.println("one");
      break;
   case "two":
      System.out.println("two");
      break;
   default:
      System.out.println("default");
   }
}
```



### 3. String 클래스의 메소드

- `length()` : 문자열 길이를 반환

- 문자열 연결시키기
  - `concat` : 문자열을 연결해서 새 문자열 참조값을 반환

```java
class StringConcat {
   public static void main(String[] args) {
      String st1 = "Coffee";
      String st2 = "Bread";
   
      String st3 = st1.concat(st2);
      System.out.println(st3);
   
      String st4 = "Fresh".concat(st3);
      System.out.println(st4);
   }
}
```

- 문자열의 일부 추출 : `substring()`

```java
String str = "abcdefg";
// 2이후 내용 => cdefg
str.substring(2);    

String str = "abcdefg";
// 2~3 내용 => cd
str.substring(2, 4);
```

- 문자열의 내용 비교
  - `==`을 사용하면 참조 값을 비교하게 됩니다.

```java
public static void main(String[] args) {
   String st1 = "Lexicographically";
   String st2 = "lexicographically";
   int cmp;
   
   // equals: 내용 비교
   if(st1.equals(st2))
      System.out.println("두 문자열은 같습니다.");
   else
      System.out.println("두 문자열은 다릅니다."); // 정답
   
   // compareTo: 일치하면 0, 사전순 앞이면 0보다 작은 값, 사전순 뒤면 0보다 큰 값
   cmp = st1.compareTo(st2);
   if(cmp == 0)
      System.out.println("두 문자열은 일치합니다.");
   else if (cmp < 0)
      System.out.println("사전의 앞에 위치하는 문자: " + st1);
   else
      System.out.println("사전의 앞에 위치하는 문자: " + st2);
   
   // 대소문자 비교를 하지 않겠다.
   if(st1.compareToIgnoreCase(st2) == 0)
      System.out.println("두 문자열은 같습니다.");
   else
      System.out.println("두 문자열은 다릅니다.");
}
```

- 기본 자료형의 값을 문자열로 바꾸기
  - 오버로딩이 다 되어있기 때문에 다양한 자료형 전달 가능
  - static 메소드

```java
double e = 2.718281;
// "2.718281"를 저장하는 스트링 인스턴스 참조값을 반환
String se = String.valueOf(e);
```

- 문자열 대상 + 연산과 += 연산

```java
System.out.println("funny" + "camp");
// 컴파이럴에 의한 자동변환 = 덧셈 연산은 concat 메소드 호출의 결과물!
System.out.println("funny".concat("camp"));

String str = "funny";
str += "camp";    // str = str + "camp"
// 자동변환
str = str.concat("camp");
```

- 문자열과 기본 자료형의 + 연산
  - concat은 문자열만 인자로 전달받을 수 있음

```java
String str = "age: " + 17;
// 컴파일러가 자동변환
String str = "age: ".concat(String.valueOf(17));
```

- concat 메소드는 이어서 호출 가능

```java
String str = "AB".concat("CD").concat("EF");
   → String str = ("AB".concat("CD")).concat("EF");
   → String str = "ABCD".concat("EF");
   → String str = "ABCDEF";
```

- 문자열 결합의 최적화를 하지 않을 경우
  - 너무 과도한 String 인스턴스 생성 => 컴파일러는 이렇게 변환하지 않습니다!

```java
String birth = "<양>" + 7 + '.' + 16; 
// 변환??
"<양>".concat(String.valueOf(7)).concat(String.valueOf('.')).concat(String.valueOf(16));
```

- 컴파일러는 효율적으로 최적화를 진행
  - `StringBuilder` : 문자열 저장 공간을 의미
  - 최종 결과물에 대한 인스턴스 생성 이외에 중간에 인스턴스를 생성하지 않습니다.
    - `toString()`를 호출한 순간 String 인스턴스를 생성 => 참조값을 반환
  - `append`는 자기 자신이 속한 인스턴스의 참조값을 반환

```java
String birth = (new StringBuilder("<양>").append(7).append('.').append(16)).toString();
```

- StringBuilder
  - 얼마든지 수정 가능

```java
public static void main(String[] args) {
   // 문자열 "123"이 저장된 인스턴스의 생성
   StringBuilder stbuf = new StringBuilder("123");
   
   stbuf.append(45678);   // 문자열 덧붙이기
   System.out.println(stbuf.toString()); // 12345678
   
   stbuf.delete(0, 2);    // 문자열 일부 삭제
   System.out.println(stbuf.toString()); // 345678
   
   stbuf.replace(0, 3, "AB");    // 문자열 일부 교체
   System.out.println(stbuf.toString()); // AB678

   stbuf.reverse();    // 문자열 내용 뒤집기
   System.out.println(stbuf.toString()); // 876BA

   String sub = stbuf.substring(2, 4);  // 일부만 문자열로 반환
   System.out.println(sub); // 6B
}
```

- StringBuffer
  - StringBuilder와 기능적으로 완전히 동일
    - 생성자를 포함한 메소드의 수 
    - 메소드의 기능
    - 메소드의 이름과 매개변수의 선언
  - 차이점
    - StringBuffer는 쓰레드에 안전하다. 
    - 따라서 쓰레드 안전성이 불필요한 상황에서 StringBuffer를 사용하면 성능의 저하만 유발하게 된다
    - 그래서 StringBuilder가 등장하게 되었다.


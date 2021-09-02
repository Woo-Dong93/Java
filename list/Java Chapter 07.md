# Java Chapter 07

### 1. 클래스의 정의와 인스턴스 생성

- 프로그램의 기본 구성
  - 데이터 : 프로그램상에서 유지하고 관리해야 할 데이터
  - 기능 : 데이터를처리하고 조작하는 기능
- 클래스
  - 연관이 되어 있는 기능들과 데이터를 묶어둔 것
  - **틀**을 정의하는 것 => 그 틀을 기반으로 인스턴스를 생성
- 인스턴스
  - 인스턴스 변수 : 클래스 내에 선언된 변수
  - 인스턴스 메소드 : 클래스 내에 정의된 메소드

- 인스턴스와 참조 변수

```java
// 참조 변수 선언
BankAccunt myAcnt1; 
BankAccunt myAcnt2;

// 새로 생성되는 인스턴스를 가리킴
myAcnt1 = new BankAccunt(); // 인스턴스 주소값 반환
myAcnt2 = new BankAccunt();
```

- 참조변수의 특성

```java
BanckAccount jang = new BankAccunt();
jang = new BankAccunt(); // 새 인스턴스를 참조

BanckAccount ref1 = new BankAccunt();
BanckAccount ref2 = ref1  // 같은 인스턴스를 참조
```

- 참조변수의 매개변수 선언

```java
BanckAccount ref = new BankAccunt();
check(ref);

public static void check(BankAccount acc) {
    acc.checkMyBalance();
}
```

- 참조변수에 `null` 대입
  - `null` : 청소부 역할, 비우거나 지우겠다는 의미

```java
BanckAccount ref = new BankAccunt();
ref = null; // ref는 누구를 참조하지 않음으로 초기화

if(ref == null) // ref가 참조하는 인스턴스나 데이터가 없다면?
```



### 2. 생성자와 String 클래스의 소개

- 자바는 문자열도 인스턴스로 저장합니다.
- "Hello" => String 클래스의 인스턴스를 가상머신이 생성 => 그 인스턴스에 문자열과 메소드를 넣어줍니다.
  - 마지막으로 그 인스턴스의 주소값을 반환
  - `System.out.println("Hello")` : 받는 인자는 String 인스턴스의 주소값이 전달

```java
String str1 = "Happy";
String str2 = "Birthday";
System.out.println(str1 + " " + str2);

// 매개변수로 String형 참조변수를 선언하여 인자를 전달할 수 있습니다.
printString(str1);
```

- 클래스 정의 모델 : 인스턴스 구분에 필요한 정보를 갖게 하자!

```java
class BankAccount {
	String accNumber; // 계좌번호
    String ssNumber; // 주민번호
    int balance = 0; // 예금잔액
    
    public int deposit(int amount) {...}
    public int withdraw(int amount) {...}
    public int checkMyBalance() {...}
}
```

- **생성자 함수** : 좋은 클래스 정의 후보를 위한 초기화 메소드
  - 딱 한번만 호출되어야 합니다.

```java
class BankAccount {
	String accNumber; // 계좌번호
    String ssNumber; // 주민번호
    int balance = 0; // 예금잔액
    
    public void initAccount(String acc, String ss, int bal){
        accNumber = acc;
        ssNumber = ss;
        balance = bal;
    }
}

BankAccount jang = new BankAccount();
jang.initAccount("1-1-1", "999999-9999999", 10000); // 초기화
```

- 자바는 **생성자 함수**를 문법으로 정의
  - 생성자의 이름은 클래스의 이름과 동일
  - 생성자는 값을 반환하지 않고 반환형도 표시하지 않습니다.

```java
class BankAccount {
	String accNumber; // 계좌번호
    String ssNumber; // 주민번호
    int balance = 0; // 예금잔액
    
    public BankAccount(String acc, String ss, int bal){
        accNumber = acc;
        ssNumber = ss;
        balance = bal;
    }
}

BankAccount jang = new BankAccount("1-1-1", "999999-9999999", 10000);
```

- 디폴트 생성자

```java
class BankAccount {
    int balance = 0; // 예금잔액
    
    // 생성자 함수를 정의하지 않으면 컴파일러가 자동으로 삽입 => 하는 일은 없다.
    public BankAccount() {
        // empty
    }
}
```

- 모든 클래스는 생성자를 직접 정의하는 것이 좋은 규칙이다!
  - 하나 이상의 데이터와 메소드가 존재
  - 데이터를 원하는 값으로 초기화 해야 하는 의무는 필요



### 3. 자바의 이름 규칙

- 클래스의 이름 규칙
  - 클래스 이름의 첫 문자는 **대문자**로 시작
  - 둘 이상의 단어가 묶여서 하나의 이름을 이룰 때, 새로 시작하는 단어는 대문자로 사용
    - Camel Case 모델
    - `Bank()`
- 메소드와 변수의 이름 규칙
  - 메소드 및 변수 이름의 첫 문자는 **소문자**로 시작
  - 둘 이상의 단어가 묶여서 하나의 이름을 이룰 때, 새로 시작하는 단어는 대문자로 사용
    - 변형된 Camel Case 모델
    - `addYouerMoney()`

- 상수의 이름 규칙
  - 상수의 이름은 **모든 문자를 대문자**로 구성
  - 둘 이상의 단어가 묶여서 하나의 이름을 이룰 때 단어 사이를 **언더바** 사용
  - `final int COLOR_RAINBOW = 7;`


# Java Chapter 06

### 1. 메소드에 대한 이해와 메소드의 정의

- 메소드는 **기능상자**!
- 자바에서 정한 규칙 : 프로그램의 시작과 종료는 **main 메소드**
- 메소드 정의와 호출
  - 입력과 출력은 있을수도 있고 없을 수도 있습니다.
  - `void` : 반환값이 없을 때 사용합니다.

```java
public static void main(String[] args) {
	System.out.println("프로그램 시작");
    hiEveryone(29);
    System.out.println("프로그램 종료");
}

public static void hiEveryone(int age) {
	System.out.println("나의 나이는 "+age);
}
```

- 값을 반환하는 메소드

```java
public static int add(int a, int b) {
	return a+b;
}
```

- return의 두 가지 의미
  - 값의 반환 없이 메소드만 종료
  - 특정 값을 반환하고 싶을 때 사용



### 2. 변수의 스코프

- 지역 => 중괄호 영역
- 지역내에 선언된 변수 : **지역변수**
- 중괄호를 빠져나가는 순간 **지역변수는 소멸**
- 매개변수도 지역변수와 동일

```java
if(...) {
	int num = 5; // 지역변수
}
```

- 같은 영역에서 동일 이름의 변수 선언 불가

```java
public static void main(String[] args) {
	int num1 = 11;
    if(true){
        int num1 = 22; // 컴파일 오류 : 자바는 허용하지 않음
    }
}
```



### 3. 메소드의 재귀 호출

```java
public static int factorial(int n) {
    if(n==1){
        return 1;
    }
    else {
        return n * factorial(n-1);
    }
}
```


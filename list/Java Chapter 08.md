# Java Chapter 08

### 1. 클래스 패스

- 다양한 경로에서 자바 가상머신이 클래스파일을 찾도록 유도할 수 있습니다.
- 현재 디렉토리 : 실행 중인 프로그램의 작업 디렉토리
  - `C:\PackageStudy>javac WhatYourName.java`
  - 현재 디렉토리를 기준으로 자바 파일을 찾습니다.
  - 그 다음 컴파일 후 3개의 Class파일이 현재 디렉토리에 생성

```java
// WhatYourName.java 파일
class AAA {}
class ZZZ {}
class WhatYourName {
    // 현재 디렉토리에서 AAA Class 파일을 찾습니다.
    AAA aaa = new AAA();
}
```

- 만약 클래스들의 위치가 다르면?
  - AAA 클래스를 못찾으니 에러가 발생 => 지시가 필요
  - 즉 클래스 패스를 지정해서 클래스를 여기서 찾아라!
- **클래스 패스** : 자바 가상머신의 클래스 탐색 경로
- `set classpath` : 클래스 패스 설정 확인
  - 환경 변수가 정의되지 않았습니다 => 아직 설정되지 않음
- `set classpath=.;C:\PackageStudy\MyClass`
  - 명령 프롬프트 한정
  - `.`: 현재 디렉토리를 의미, 명시적으로 추가해야 자기 위치에서 먼저 찾는 것을 유지

- 절대경로 vs 상대경로
  - `C:\PackageStudy > set classpath=.;.\MyClass` : 현제 디렉토리를 기준으로 상대경로 사용
  - `C:\PackageStudy > set classpath=.;C:\PackageStudy\MyClass` : 루트 디렉토리를 시작으로 절대경로 ( 잘 사용하지 않음 )

- 환경 변수를 이용해 클래스 패스를 완전히 고정시킬 수 있습니다.
  - 좋은 방법은 아니다!



### 2. 패키지의 이해

- 패키지 선언이 필요한 상황의 연출
  - 공간에서의 충돌 : 동일 이름의 클래스 파일을 같은 위치에 둘 수 없다.
  - 접근 방법에서의 충돌 : 인스턴스 생성방법에서 두 클래스에 차이가 없다.
  - 그럼 동일한 클래스를 사용하고 싶을 때 방법은 ?
    - 디렉토리를 다르게 하기 => 경로문제 해결
    - 클래스의 이름도 다르게 수정
- 패키지 선언
  - 패키지 이름은 모두 소문자로 구성
  - 인터넷 도메인 이름의 역순으로 이름을 구성
  - 이름 끝에 클래스를 정의한 주체 또는 팀의 이름 추가
  - **패키지의 선언을 한 클래스파일은 해당 디렉토리에 존재**해야 합니다.
  - 자바 가상머신은 클래스 패스를 기준으로 `com`이라는 Path를 찾습니다. 

```java
package com.wxfx.smart;
public class Circle {
      double rad;
      final double PI;

      public Circle(double r) { . . . }
      public double getArea() { . . . }
}

package com.fxmx.simple;
public class Circle { 
      double rad;
      final double PI;
    
      public Circle(double r) { . . . }
      public double getPerimeter() { . . . }
}

com.wxfx.smart.Circle c1 = new com.wxfx.smart.Circle(3.5);
com.fxmx.simple.Circle c2 = new com.fxmx.simple.Circle(5.5);
```

- 패키지 선언이 된 소스파일 컴파일 방법
  - `C:\PackageStudy>javac -d . src\circle1\Circle.java`
  - 패키지 정보를 바탕으로 알아서 디렉토리를 생성 후 클래스 파일을 생성합니다.

![](../img/17.png)

- 클래스 하나에 대한 `import` 선언
  - 인스턴스 생성할 때 패키지를 명시해주지 않아도 됩니다.
  - 똑같은 클래스를 import 선언하면 안됩니다.
  - `import com.wxfx.smart.*;` : 전체를 `import`하는 방식
    - 자주 사용하지 않는 방식 => 충돌이 생길수도...

![](../img/18.png)


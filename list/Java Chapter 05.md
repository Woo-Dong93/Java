# Java Chapter 05

### 1. if 그리고 else

- if문 괄호 안에는 `true` or `false`를 반환하는 값이 존재해야 합니다.
- if문 속에 문장이 하나일 경우 중괄호 생략 가능

```java
if(n1 < n2){
	System.out.println("true");
}

if(n1 < n2)
	System.out.println("true");
```

- else문은 if문의 조건이 false일 때 실행되는 영역
- else문 속에 문장이 하나일 경우 중괄호 생략 가능

```java
if(n1 < n2){
	System.out.println("true");
} else {
	System.out.println("false");
}
```

- if~else if~else문

```java
if(n1 < n2){
	System.out.println("true");
} else if(...) {
	System.out.println("true");
} else {
	System.out.println("false");
}
```

- 삼항 연산자
  - `조건` ? `수1` : `수2`
    - `true` : `수1`
    - `false`: `수2`

```java
int result = (num1 > num2) ? num1 : num2;
int result = (num1 > num2) ? (num1-num2) : (num2-num1);
```



### 2. Switch와 Break

- 기본 구성
  - `default` : case에 아무도 걸리지 않을 때 마지막에 실행

```java
switch(n) {
    case 1:
        ...
	case 2:
        ...
    case 3:
        ...
   default:
        ...            
}
```

- break문을 추가하면 switch문을 빠져나갈 수 있습니다.

```java
switch(n) {
    case 1:
        ...
        break;
	case 2:
        ...
        break;
    case 3:
        ...
        break;
   default:
        ...            
}
```

### 

### 3. for, while 그리고 do~while

- 특정 문장을 조건을 바탕으로 **반복** 수행( 횟수와 만족 )
- while문
  - 먼저 조건 검사 후 그 결과가 `true`면 중괄호 영역 실행

```java
while(num < 5){
	System.out.println("good");
	num++;
}
```

- do~while문
  - 일단 중괄호 영역 실행
  - 그리고 조건 검사 후 결과가 `true`면 반복 수행

```java
do {
	System.out.println("good");
	num++;
} while(num < 5);
```

- for 문
  - 반복의 횟수를 세기 위한 변수
  - 반복의 조건
  - 반복의 조건을 무너뜨리기 위한 연산

```JAVA
for(int num = 0; num < 5; num++){
	System.out.println("good");
}
```



### 4. Braek 와 Continue

- `break` : 모든 반복분에서 빠져나가는 용도로 활용 가능

```java
while(n < 100) {
	if(x == 20)
		break;
    ...
}
```

- `continue` : 위에서 부터 계속(조건 검사) 진행

```java
while(n < 100) {
	if(x == 20)
		continue;
    ... // continue가 실행되면 다음 문장들은 모두 생략 후 조건 검사 다시 실행
}
```

- 무한 루프
  - 빠져나가는 특정 조건이 무조건 존재 => `break`문 활용

```java
for( ; ; ){
	...
}

while(true) {
    ...
}

do {
    ...
} while(true)
```



### 5. 반복문의 중첩

- 반복문들 끼리 서로 중첩이 가능합니다.

```java
for(int i = 0; i < 5; i++){
	System.out.println("good");
    for(int j = 0; j < 5; j++){
		System.out.print("["+i+", "+j+"]");
	}
    System.out.print('\n');
}
```


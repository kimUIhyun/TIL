# Ch2 변수-1

```java
public class VerEx1 {
    public static void main(String[] args) {
        int year = 0;
        int age = 14;

        System.out.println(year);
        System.out.println(age);

        year = age + 2000;
        age = age + 1;

        System.out.println(year);
        System.out.println(age);
    }
}
```

- println은 출력 할 때 형식 지정자 필요 없이 변수 그대로 출력됨

```java
public class VarEx2 {
    public static void main(String[] args) {
        int x = 10, y = 20;
        int tmp = 0;

        System.out.println("x:"+ x + " y:"+ y);

        tmp = x;
        x = y;
        y = tmp;

        System.out.println("x:"+ x + " y:" + y);
    }
}
```
- 덧셈연산자 + 는 두 값을 더하기도 하지만 문자열과 숫자를 하나로 결합하기도 함

### 명명규칙
- 대소문자 구분
- 예약어는 사용하면 안 됨
- 숫자로 시작해선 안 됨
- 특수문자는 '_'와 '$'만 허용


### 변수의 타입
- 기본형
    - 논리형 (boolean), 문자형 (char), 정수형 (byte, short, int, long), 실수형 (float, double)
    - 계산을 위한 실제 값을 저장

- 참조형
    - 객체의 주소를 저장
    - 변수의 타입으로 클래스의 이름을 사용
        - ex) Date today = new Date(); -> Date 타입 객체 생성 후 today 에 주소 저장
    - C언어와 달리 참조형 변수 간의 연산을 할 수 없음


### 기본 자료형의 크기
- boolean: 1바이트
- char: 2바이트
- byte: 1바이트
- short: 2바이트
- int: 4바이트
- long: 8바이트
- float: 4바이트, 정밀도 낮음
- double: 8바이트, 정밀도 높음

### 상수
- 상수는 변수와 마찬가지로 값을 저장할 수 있는 공간이지만 한 번 값을 저장하면 변경이 불가능함
- 그러므로 선언과 동시에 초기화 해줘야함
- 변수의 타입 앞에 키워드 final 을 붙여 선언
    - ex) final int MAX_SPEED = 10;



## 리터럴
위와같이 상수라는 용어를 정의 했기 때문에 본래 상수의 의미인 값 자채는 리터럴이라고 칭함
- 리터럴에 접미사를 붙여 정수형과 실수형 안의 타입들을 구분함
    - L: 정수형 long
    - f: 실수형 float
    - int와 double은 기본 자료형이라 생략 가능
    - byte와 short는 별도의 접미사 없음

- 리터럴에 접두사를 붙여 진법 표현
    - 0b : 2진수
    - 0 : 8진수
    - 0x : 16진수

- 자료형과 리터럴 타입이 다르면 문법 오류
    - 타입이 달라도 저장범위가 넓은 타입에 좁은 타입의 값을 저장하는 것은 허용됨

### 문자와 문자열 리터럴
- 문자 리터럴은 ' ' 이다
    - ex) char ch = 'J';
- 문자열 리터럴은 " " 이다
    - ex) String name = "Java";
    - String은 클래스이므로 객체를 생성하는 연산자 new를 사용해야 하지만 String은 위와 같은 표현도 허용함
    - 덧셈 연산자로 문자열을 결합할 수 있음
        - ex) String name = "Ja" + "va";
        - ex) String str = name + 8.0;

### 덧셈 연산자의 문자열 결합

```java
public class StringEx {
    public static void main(String[] args) {
        String name = "Ja" + "va";
        String str = name + 8.0;

        System.out.println(name);
        System.out.println(str);
        System.out.println(7 + " ");
        System.out.println(" " + 7);
        System.out.println("" + "");
        System.out.println(7 + 7 + ""); //왼쪽에서 오른쪽으로 연산하기 때문에 피연산자 순서에 따라 결과 다름
        System.out.println("" + 7 + 7);
    }
}
```
- 덧셈 연산자는 피연산자중 하나가 문자열이면 나머지 하나도 문자열로 변환한 다음 문자열을 결합합
- 덧셈 연산자는 왼쪽에서 오른쪽으로 연산하는 이항 연산자이기 때문에 결합 순서에 따라 결과가 달라짐 주의

## printf() - 형식화된 출력
지시자(형식 지정자) 를 사용하여 값을 변환하지 않고 다른 형식으로 출력 가능

- ex) print("age:%d\n", age);
- %b : boolean
- %d : 10진 정수
- %o : 8진 정수
- %x %X : 16진 정수
- %f : 부동 소수점 float 실수
- %e %E : 지수 표현식
- %c : 문자
- %s : 문자열

```java
public class PrintfEx1 {
    public static void main(String[] args) {
        byte b = 1;
        short s = 2;
        char c = 'A';

        int finger = 10;
        long big = 100_000_000_000L; // 리터럴에 _사용 가능, 리터럴 접미사 L
        long hex = 0xFFFF_FFFF_FFFF_FFFFL;

        int octNum = 010;
        int hexNum = 0x10;
        int binNum = 0b10;

        System.out.printf("b=%d%n", b);
        System.out.printf("s=%d%n", s);
        System.out.printf("c=%c, %d %n", c, (int)c); //int로 형변환
        System.out.printf("finger=[%5d]%n", finger); // 앞에 공백
        System.out.printf("finger=[%-5d]%n", finger); // 뒤에 공백
        System.out.printf("finger=[%05d]%n", finger); // 앞에 0으로 채우기
        System.out.printf("big=%d%n", big);
        System.out.printf("hex=%#x%n", hex); // #은 접두사까지 표현 (16진수 0x)
        System.out.printf("octNum=%o, %d%n", octNum, octNum);
        System.out.printf("hexNum=%x, %d%n", hexNum, hexNum);
        System.out.printf("bigNum=%s, %d%n", Integer.toBinaryString(binNum), binNum);
    }
}

```
- C와 달리 자바에선 char타입의 값을 %d로 출력할 수 없음 형변환 해줘야함

```java
public class PrintfEx2 {
    public static void main(String[] args) {
        String url = "www.naver.com";

        float f1 = .10f;
        float f2 = 1e1f;
        float f3 = 3.14e3f;
        double d = 1.23456789;

        System.out.printf("f1=%f, %e, %g%n", f1, f1, f1); //%g는 값을 간략하게 표기할 때 사용
        System.out.printf("f2=%f, %e, %g%n", f2, f2, f2);
        System.out.printf("f3=%f, %e, %g%n", f3, f3, f3);

        System.out.printf("d=%f%n", d);
        System.out.printf("d=%14.10f%n", d);

        System.out.printf("[1234567890]%n");
        System.out.printf("[%s]%n", url);
        System.out.printf("[%20s]%n", url);
        System.out.printf("[%-20s]%n", url); // 왼쪽 정렬
        System.out.printf("[%.8s]%n", url); // 왼쪽에서 8글자만 출력
    }
}
```
- %g는 값을 간략하게 표현할 때 사용
- %f는 기본적으로 소수점 아래 6자리까지만 출력하고 아래 7자리에서 반올림 함
  - %14.10f 처럼 (전체자리수).(소수점아래자리수) 로 지정해서 출력할 수 있음
- %20s 는 문자열 출력 공간 20만큼 확보뒤 출력하고 공간이 남으면 우측 정렬함
    - %-20s 는 반대로 죄측 정렬함

## Scanner - 화면에서 입력 받기

```java
import java.util.*;

public class ScannerEx {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.printf("두자리 정수를 하나 입력해주세요.>");
        String input = scanner.nextLine();
        int num = Integer.parseInt(input); // 입력 받은 문자열을 숫자로 반환

        System.out.println("입력내용 : "+input);
        System.out.printf("num=%d%n", num);
    }
}
```

- Scanner 클래스 사용하기 위해 import java.util.*/ 로 뭔가 불러옴
- Scanner 클래스의 객체를 생성
    - 객체 안의 nextLine() 메서드를 사용해서 문자열을 입력 받음
    - 입력 받은 문자열을 Integer.parseInt() 라는 메서드를 이용하여 정수형 변수 num에 저장
    - 애초에 nextInt()나 nextFloat()등의 메서드를 사용해도 되지만 연속적으로 받기가 까다로워서 문자열로 한번에 받고 형변환 함
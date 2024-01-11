# Ch9 java.lang 패키지와 유용한 클래스

## 1.3 StringBuffer 클래스와 StringBuilder 클래스
내부적으로 문자열 편집을 위한 버퍼를 가지고있음 나머지는 String과 유사함

### StringBuffer의 생성자
- 인스턴스를 생성할 때 적절한 길이의 char형 배열이 생성되고 이를 버퍼로 사용
- 생성자 StringBuffer(int length)룰 사용하여 버퍼의 길이를 지정하고 지정하지 않으면 16으로 지정됨
- 생성자 StirngBuffer(String str)은 지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성함
- 문자열의 길이가 버퍼의 크기보다 커지는 경우 버퍼의 크기를 늘리는 작업이 필요함
  - 더 큰 배열을 만들고 이전 배열의 값을 복사

### StringBuffer의 문자열 변경
append() 메서드를 이용하여 문자열 뒤에 문자열을 추가할 수 있음
```java
// 정확하진 않지만 대충 이런 형태임
StringBuffer append(StringBuffer s) {
    //...
}
```
위와 같이 반환형이 StringBuffer이기 때문에
```java
StringBuffer sb = new StringBuffer("abc");
sb.append("123");
StringBuffer sb2 = sb.append("zz");
sb2.append("Hello").append(" World");
System.out.println(sb); // abc123zzHelloWorld
System.out.println(sb2);    // abc123zzHelloWorld
```
이런식으로 다양하게 사용할 수 있음

### StringBuffer의 비교
StringBuffer 에서는 equals 메서드가 오버라이딩 되어있지 않기 때문에 == 연산자와 같은 기능을 함
- String의 equals 처럼 문자열 내용을 비교하려면
  - StringBuffer의 toString()을 호출해 String 인스턴스를 얻는다
  - 그 String 인스턴스로 equals 메서드를 사용하여 비교한다

### StringBuffer 클래스의 생성자와 메서드

<details>
<summary>1. 생성</summary>
<div>

|메서드/설명|예제|결과|
|------|---|---|
|StringBuffer() <br> 16문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스 생성|StringBuffer sb = new StringBuffer();|sb = ""|
|StringBuffer(int length) <br> 지정된 개수의 문자를 담을 수 있는 버퍼를 가진 StringBuffer 인스턴스 생성|StringBuffer sb = new StringBuffer(10);|sb = ""|
|StringBuffer(String str) <br> 지정된 문자열 값(str)을 가진 StringBuffer 인스턴스 생성|StringBuffer sb = new StringBuffer("Hi);|sb = "Hi"|

</div>
</details>

<details>
<summary>2. 조회</summary>
<div>

|메서드/설명|예제|결과|
|------|---|---|
|int capacity() <br> StringBuffer인스턴스의 버퍼크기를 반환|StringBuffer sb = new StringBuffer(100); <br> sb.append("abcd"); <br> int bufferSize = sb.capacity();|bufferSize = 100|
|int length() <br> StringBuffer인스턴스의 문자열 길이를 반환|StringBuffer sb = new StringBuffer(100); <br> sb.append("abcd"); <br> int stringSize = sb.length();|stringSize = 4|
|char charAt(int index) <br> 지정된 위치(index)에 있는 문자를 반환|StringBuffer sb = new StringBuffer("abc"); <br> char c = sb.charAt(2);|c = 'c'|

</div>
</details>

<details>
<summary>3. 편집</summary>
<div>

|메서드/설명|예제|결과|
|------|---|---|
|StringBuffer append(boolean b) <br> StringBuffer append(char c) <br> StringBuffer append(char[] str) <br> StringBuffer append(double d) <br> StringBuffer append(float f) <br> StringBuffer append(int i) <br> StringBuffer append(long l) <br> StringBuffer append(Object obj) <br> StringBuffer append(String str) <br> 매개변수로 입력된 값을 문자열로 변환하여 StringBuffer 인스턴스의 문자열 뒤에 덧붙임|StringBuffer sb = new StringBuffer("abc"); <br> StringBuffer sb2 = sb.append(true); <br> sb.append('d').append(10.0f); <br> StringBuffer sb3 = sb.append("ABC").append(123);|sb = "abctrue10.0ABC123" <br> sb2 = "abctrue10.0ABC123" <br> sb3 = "abctrue10.0ABC123"|
|StringBuffer insert(int pos, boolean b) <br> StringBuffer insert(int pos, char c) <br> StringBuffer insert(int pos, char[] str) <br> StringBuffer insert(int pos, double d) <br> StringBuffer insert(int pos, float f) <br> StringBuffer insert(int pos, int i) <br> StringBuffer insert(int pos, long l) <br>StringBuffer insert(int pos, Object obj) <br> StringBuffer insert(int pos, String str) <br> 매개변수로 입력된 값을 문자열로 변환하여 지정된 위치(pos)에 추가 (0부터 시작)|StringBuffer sb = new StringBuffer("0123456"); <br> sb.insert(4, '.');|sb = "0123.456"|
|StringBuffer delete(int start, int end) <br> 시작위치(start)부터 끝 위치(end)사이에 있는 문자 제거 [start,end)|StringBuffer sb = new StringBuffer("0123456"); <br> StringBuffer sb2 = sb.delete(3,6);|sb = "0126" <br> sb2 = "0126"|
|StringBuffer deleteCharAt(int index) <br> 지정된 위치(index)에 있는 문자 제거|StringBuffer sb = new StringBuffer("0123456"); <br> StringBuffer sb2 = sb.deleteCharAt(3);|sb = "012456" <br> sb2 = "012456"|
|StringBuffer replace(int start, int end, String str) <br> 시작위치(start)부터 끝 위치(end)사이에 있는 문자들을 주어진 문자열로 대치 [start,end)|StringBuffer sb = new StringBuffer("0123456"); <br> sb = sb.replace(3,6,"AB");|sb = "012AB6"|
|StringBuffer reverse() <br> 문자열의 순서를 거꾸로 나열함|StringBuffer sb = new StringBuffer("0123456"); <br> sb = sb.reverse();|sb = "6543210"|
|StringBuffer setCharAt(int index, char ch) <br> 지정된 위치(index)에 있는 문자를 주어진 문자로 대치|StringBuffer sb = new StringBuffer("0123456"); <br> sb = sb.setCharAt(5, 'o');|sb = "01234o6"|
|StringBuffer setLength(int newLength) <br> 문자열의 길이를 지정된 길이로 변경. 나머지 공간은 널문자('\n0000')로 채움|StringBuffer sb = new StringBuffer("0123456"); <br> sb.setLength(5); <br> StringBuffer sb2 = new StringBuffer("0123456"); <br> sb2.setLength(10); <br> String str = sb2.toString().trim()|sb = "01234" <br> sb2 = "012456     " <br> str = "0123456"|

</div>
</details>

<details>
<summary>4. 변환</summary>
<div>

|메서드/설명|예제|결과|
|------|---|---|
|String toString() <br> StringBuffer인스턴스의 문자열을 String으로 반환|StringBuffer sb = new StringBuffer("0123456"); <br> String str = sb.toString();|str = "0123456"|
|String substring(int start) <br> tring substring(int start, int end) <br> 지정된 범위 내의 문자열을 String으로 뽑아서 반환 [start, end)|StringBuffer sb = new StringBuffer("0123456"); <br> String str = sb.substring(3); <br> String str2 = sb.substring(3, 5)|str = "3456" <br> str2 = "34"|

</div>
</details>

### StringBuilder
StringBuffer는 멀티쓰레드에 안전하도록 동기화 되어있기 때문에 동기화가 StringBuffer의 성능을 저하시킴 (Ch13)
멀티쓰레드로 작성된 프로그램이 아니라면 괜히 성능만 떨어지게 됨  
StringBuilder는 쓰레드 동기화만 빠진 StringBuffer임

## 1.4 Math 를래스
기본적인 수학계산에 유용한 메서드들 가짐
- 멤버는 모두 static이고 메서드들과 두개의 상수(자연로그 밑, 원주율)만을 정의함
- 인스턴스 멤버가 없어 객체를 생성할 필요가 없기 때문에 생성자는 private임

### 올림, 버림, 반올림  

#### 올림
ceil() 메서드는 ()안의 수를 소수점 첫째 자리에서 올림하고 실수형을 반환함

#### 버림
floor() 메서드는 ()안의 수를 소수점 첫째 자리에서 버림하고 실수형을 반환함

#### 반올림

round() 메서드는 소수점 첫째 자리에서 반올림을 해서 정수값(long)을 반환함  원하는 자리 수에서 반올림을 하려면 다음과 같이 해야함
- 원하는 자리수가 소수점 첫째 자리에 오게끔 10을 곱함
  - 90.7552 * 100 -> 9075.52
- 그 결과에 Math.round() 사용
  - Math.round(9075.52) -> 9076L
- 결과 값을 다시 원래 소수점으로 변경
  - 9076L / 100.0 -> 90.76
- 이 때 곱하거나 나누는 수가 정수형이냐 실수형이냐에 따라 값이 달라짐

rint() 메서드는 round() 메서드와 동일하지만 double형으로 반환함  
두 정수의 정 가운데 있는 값은 가장 가까운 짝수를 반환함 (rint(-1.5) -> -2.0)

### 예외를 발생시키는 메서드
메서드 이름에 'Exact'가 포함되어있으면 정수형의 연산에서 발생할 수 있는 오버플로우를 감지하고 예외를 발생시킴
```java
int addExact(int x, int y);
int subtractExact(int x, int y);
int multiplyExact(int x, int y);
int incrementExact(int a);
int decrementExact(int a);
int negateExact(int a); // 부호 전환시 보수를 취했을 때 정수형의 최대값이면 +1 하면 오버플로우 발생함
int toIntExact(long L);
```
오버플로우가 발생해서 예외가 발생하면 형변환을 해주고 연산을 해주는 등 예외 처리가 필요함

### 삼각함수, 지수, 로그
- double sqrt() : 제곱근 반환
- double pow() : 제곱 반환
- double cos(PI/4) : cos(PI/4)
- double sin(PI/4) : sin(PI/4)
- double toRadians() : 괄호의 각도를 라디안으로 변환
- double atan2() : 직각삼각형의 두 변의 길이를 주면 끼인각의 크기를 라디안으로 반환함
- double log10() : 괄호안의 수를 상용로그 취함

### StrictMath 클래스
Math 클래스는 JVM이 설치된 OS의 메서드를 호출해서 사용하므로 부동소수점 처리 방식 같은 것이 OS마다 다를 수 있으므로 성능을 조금 포기하고 일관된 결과를 얻을 수 있는 StrictMath 클래스를 재공함

### Math 클래스의 메서드
- abs() : 절대값 반환
- ceil() 소수점 첫째자리에서 올림
- floor(): 소수점 첫째자리에서 버림
- round(): 소수점 첫째자리에서 반올림. long형으로 반환
- rint() : 소수점 첫째자리에서 반올림. double형으로 반환. 두 정수의 정가운데 값은 짝수를 반환
- max() : 두 값중 큰 값
- min() : 두 값중 작은 값
- random() : [0.0, 1.0) 범위의 임의의 double 값 반환
  
## 1.5 wrapper 클래스
기본형 값을 객체로 다루기 위한 클래스. 기본형에 맞춰 총 8개

![Wrapper Class](<Wrapper Class.png>)
래퍼 클래스들은 내부적으로 알맞는 기본형에 해당하는 멤버를 가지고 있기 때문에 생성할 때 알맞는 자료형을 전달해줘야 함
- 래퍼 클래스들은 모두 equals()가 오버라이딩되어 있어서 주소값이 아닌 객체가 가진 값을 비교함
- toString()도 역시 오버라이딩되어 있음
- 상수 멤버로 MAX_VALUE, MIN_VALUE, SIZE, BYTES, TYPE 등을 공통적으로 가지고있음

### Number 클래스
숫자를 멤버변수로 갖는 래퍼 클래스들의 조상. 추상 클래스임

![Wrapper Class2](<Wrapper Class2.png>)
- BigInter, BigDecimal : long과 double로도 다룰 수 없는 큰 범위의 정수와 실수를 나타내기 위한 클래스
- Number 클래스는 객체가 가지고 있는 값을 숫자와 관련된 기본형으로 변환하여 반환하는 메서드들을 가지고있음

### 문자열 숫자로 변환하기
```java
  int i = new Integer("100").iniValue();
  int i = Integer.parseInt("100");  // 반환형이 기본형. 주로 이 방식 많이 사용
  Integer i = Integer.valueOf("100");   // 반환형이 wrapper 클래스임
```
- JDK1.5 부터 오토박싱 기능 덕분에 반환값이 기본형이나 래퍼 클래스이나 차이가 없어져서 형 구분 없이 valueOf()를 쓰는 것도 괜찮음 (조금 느리긴 함)
- 두번째 인자로 몇진수로 읽을지 정수로 전달하면 해당하는 진법으로 문자열을 읽음

### 오토박싱 & 언박싱
원래는 기본형과 참조형 간의 연산이 불가능해 기본형을 래퍼 클래스 객체로 만들어 연산을 했어야 했는데 오토박싱 기능이 추가되면서 자동으로 변환해줌 (반대는 언박싱)
```java
int i = 5;
Integer iObj = new Integer(7);

int sum = i + iObj  // 컴파일러가 int sum = i + iObj.intValue(); 로 코드 추가
```

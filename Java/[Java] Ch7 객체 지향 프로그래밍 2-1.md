# Ch7 객체 지향 프로그래밍 2

## 상속
기존 클래스를 재사용하여 새로운 클래스를 작성하는 것
- 프로그램의 중복을 방지해 안정성을 높일 수 있음
- 공통적인 부분은 조상 클래스에서만 바꾸거나 추가하면 되기 때문에 유지 보수 효율이 높아짐

자손 클래스 extends 조상 클래스  
하위 클래스 extends 상위 클래스  
파생된 클래스 extends 기반 클래스  

```java
class Child extends Parent {
    //...
}
```
- 생성자와 초기화 블럭은 상속되지 않고 멤버만 상속됨
- 모든 멤버가 상속 되기 때문에 자손 클래스의 멤버 개수는 조상 클래스보다 항상 같거나 많다
- 상속의 상속, 간접 상속도 가능함

```java
class Tv {
    boolean power;
    int channel;

    void power() {power = !power;}
    void channelUp() {++channel;}
    void channelDown() {--channel;}
}

class CaptionTv extends Tv {    // Tv 클래스 상속
    boolean caption;
    void displayCaption(String text) {
        if (caption)
            System.out.println(text);
    }
}

public class CaptionTvTest {
    public static void main(String[] args) {
        CaptionTv ctv = new CaptionTv();    // 자식 클래스 인스턴스 생성
        ctv.channel = 10;   // 자식 클래스인 CaptionTv의 인스턴스로 조상 클래스 Tv의 멤버에 접근
        ctv.channelUp();
        System.out.println(ctv.channel);
        ctv.displayCaption("Hello, World");
        ctv.caption = true;
        ctv.displayCaption("Hello, World");
    }
}
```

### 클래스간의 포함 관계
하나의 거대한 클래스를 작성하는 것 보다 단위별로 클래스를 나눠 큰 클래스 안에서 멤버 처럼 사용하고 관리
```java
class Car {
    Engine e = new Engine();    // Engine 에 관한것은 Engine 클래스에서 따로 관리 해주면 됨
    Door[] d = new Door[4];
}
```

### 클래스간의 관계 설정하기 (상속 or 포함)
- 상속관계 : is, ~은 ~이다
- 포함관계 : has, ~은 을 가지고있다

### 관계 설정 예시
<details>
<summary>예시1 - 원과 삼각형 그리기</summary>
<div>

```java
public class DrawShape {
    public static void main(String[] args) {
        Point[] p = {new Point(100, 100),
                new Point(140, 50),
                new Point(200, 100)
        };

        Triangle t = new Triangle(p);
        Circle c = new Circle(new Point(150, 150), 50);

        t.draw();
        c.draw();
    }
}

class Shape {
    String color = "black";
    void draw() {   // 오버라이딩
        System.out.printf("[color=%s]%n", color);
    }
}

class Point {
    int x;
    int y;
    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    Point() {
        this(0,0);
    }

    String getXY() {
        return "("+x+","+y+")";
    }
}

class Circle extends Shape {    // Shape을 상속
    Point center;   // Point를 포함, 원의 중심
    int r;

    Circle() {
        this(new Point(0,0), 100);
    }
    Circle(Point center, int r) {
        this.center = center;
        this.r = r;
    }

    void draw() {   // 오버라이딩
        System.out.printf("[center=(%d, %d), r=%d, color=%s]%n",
                                        center.x, center.y, r, color);
    }
}

class Triangle extends Shape {  // Shape을 상속
    Point[] p = new Point[3];   // Point를 포함, 삼각형의 세 점

    Triangle(Point[] p) {
        this.p = p;
    }

    void draw() {   // 오버라이딩
        System.out.printf("[p1=%s, p2=%s, p3=%s, color=%s]%n", p[0].getXY(), p[1].getXY(), p[2].getXY(), color);
    }
}
```
- 원과 삼각형은 도형이다 -> 상속
- 원과 삼각형은 점을 가지고있다 -> 포함

</div>
</details>

<details>
<summary>예시2 - 카드 뭉치</summary>
<div>

```java
public class DeckTest {
    public static void main(String[] args) {
        Deck d = new Deck();
        Card c = d.pick(0);
        System.out.println(c);  // c.toString() 호출

        d.shuffle();
        c = d.pick(0);
        System.out.println(c);  // c.toString() 호출
    }
}

class Deck {
    final int CARD_NUM = 52;
    Card cardArr[] = new Card[CARD_NUM];    // Card 객체 배열 포함

    Deck() {
        int i = 0;

        for(int k=Card.KIND_MAX; k>0; k--)
            for(int n=0; n<Card.NUM_MAX; n++)
                cardArr[i++] = new Card(k, n+1);
    }

    Card pick(int index){
        return cardArr[index];
    }

    Card pick() {
        int index = (int)(Math.random() * CARD_NUM);
        return pick(index);
    }

    void shuffle() {
        for(int i=0; i<cardArr.length; i++) {
            int r = (int)(Math.random() * CARD_NUM);
            Card temp = cardArr[i];
            cardArr[i] = cardArr[r];
            cardArr[r] = temp;
        }
    }
}

class Card {
    static final int KIND_MAX = 4;
    static final int NUM_MAX = 13;

    static final int SPADE = 4;
    static final int DIAMOND = 3;
    static final int HEART = 2;
    static final int CLOVER = 1;
    int kind;
    int number;
    Card() {
        this(SPADE, 1);
    }

    Card(int kind, int number) {
        this.kind = kind;
        this.number = number;
    }

    public String toString() {  // Object클래스에 정의
        String[] kinds = {"", "COLVER", "HEART", "DIAMOND", "SPADE"};
        String numbers = "0123456789XJQK";
        return "kind : " + kinds[this.kind] + ", number : " + numbers.charAt(this.number);
    }
}
```
- 덱은 카드를 가지고있다 -> 포함
- 참조변수 c를 출력할 때 자동으로 c.toString()이 호출 되면서 인스턴스의 정보를 문자열로 변환함
  - 이는 모든 클래스의 조상인 Object 클래스에 정의 되어있음. Ch9에서 다룰 예정

</div>
</details>

### 단일 상속
C++ 에서는 다중 상속이 허용 됐지만 Java에서는 오직 단일 상속만이 허용된다
- 다중 상속을 받을 경우 서로 다른 조상에 같은 이름의 인스턴스 변수나 메서드가 있을 경우 구별이 안되고 이름을 변경한다 하면 연쇄적으로 변경을 해줘야하기 때문에 안정성 문제로 지원하지 않음
- 포함 관계를 적절히 이용하면 여러 클래스를 재사용해 다룰 수 있음
```java
class Tv{  // VCR의 멤버와 이름 중복되는 멤버 가짐
    boolean power;
    int channel;
    
    void power() { power = !power; }
    //...
}

class VCR{  // Tv의 멤버와 이름 중복되는 멤버 가짐
    boolean power;
    int counter = 0;
    
    void power() { power = !power; }
    //...
}

class TVCR extends Tv { // Tv 클래스는 상속
    VCR vcr = new VCR(); // VCR 클래스는 포함

    void power() { vcr.power(); } // VCR 클래스의 인스턴스로 VCR 멤버 함수 사용
}
```

### Object 클래스 - 모든 클래스의 조상
- 다른 클래스로부터 상속 받지 않는 모든 클래스들은 컴파일러를 통해 자동적으로 Object 클래스로부터 상속 받음  
- 다른 클래스로부터 상속 받는 클래스들도 간접 상속으로 인해 결국 최상위 조상은 Object임  

Object의 멤버에 관해선 Ch9에서 다룰 예정


## 오버라이딩
조상 클래스로부터 상속받은 메서드의 내용을 변경하는(덮어씌우는) 것
- 메서드의 내용만을 새로 작성하는 것이기 때문에 이름, 매개변수, 반환타입이 같아야한다
  - 단 반환타입을 자손 클래스로 변경하는 것은 허용됨
- 조상 클래스보다 좁은 범위의 접근 제어자는 사용할 수 없음
- 조상 클래스의 메서드보다 많은 예외를 선언할 수 없음
- 인스턴스 메서드와 static 메서드에서 서로 변경 될 수 없음
  - static 메서드와 같은 이름을 가진 메서드를 자손 클래스에서 정의하는 것은 어짜피 static 메서드는 클래스 이름으로 구별 되므로 별개의 static 메서드를 정의한 것일 뿐 오버라이딩이 아님, static멤버들은 정의된 클래스에 묶여있는 것임

### super
모든 인스턴스는 자신의 주소를 가지는 참조변수 this와 super를 가짐  
여기서 super는 조상 인스턴스 주소를 가지고 조상의 멤버에 접근할 때 사용함
- 조상의 멤버와 자손의 멤버중 이름이 같은 것이 있으면 super로 구분 가능함
```java
class 2DPoint {
    int x;
    int y;
    void showPoint() { System.out.println(x + ", " + y); }
}

class 3DPoint extends 2DPoint {
    int z;
    void showPoint() {  // 오버라이딩
        super.showPoint();      // super로 조상의 메서드를 호출
        System.out.println(", " + z); 
    }
}
```
- 이와 같이 오버라이딩시에 공통되는 기능은 super로 조상의 메서드를 호출해 유지 보수가 편하게 만들 수도 있음

### super()
this()가 인스턴스 자신의 생성자를 가리키는 것 처럼 super()는 조상 인스턴스의 생성자를 가리킴  
***"자손의 생성자에서 super()가 존재하지 않으면 컴파일러가 자동으로 super()를 삽입함"***
- 조상 클래스에 기본 생성자가 있으면 문제가 안되지만 기본 생성자가 없는 경우 문제가 발생하므로 super()로 알맞는 조상의 생성자를 지정해주어야 함
- 이는 계속 거슬러 올라가서 Object()까지 거슬러 올라감

```java
class Point {
    int x, y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Point3D extends Point {
    int z;

    Point3D() {
        this(100, 200, 300);
    }
    Point3D(int x, int y, int z) {
        super(x, y);    // super 사용
        this.z = z;
    }
}
```
- 생성자 Point3D() 실행시 실행순서: Point3D() -> Point3D(int x, int y, int z) -> Point(int x, int y) -> Object()

## Package와 Import
### 패키지
클래스의 묶음, 디렉토리
- 클래스의 풀네임은 그의 디렉토리(패키지)를 포함한 것임
  - String 클래스의 실제 이름은 java.lang.String (java.lang 패키지에 속한 String 클래스)
  - System 클래스도 java.lang.System 임 (java 패키지의 하위 패키지인 lang 패키지에 속한 System 클래스)
- 모든 클래스는 반드시 하나의 패키지에 속해야 함

### 패키지의 선언
```java
package 패키지명;   // 클래스나 인터페이스 소스파일 맨 위
```
- 소스파일에서 첫 번째 문장에 위치해야하고 하나의 소스파일에 한번만 선언될 수 있음
- 해당 소스파일에 속한 클래스나 인터페이스는 해당 패키지에 속하게 됨
- 패키지를 선언하지 않은 경우 '이름없는 패키지'에 자동적으로 속하게 됨
- 클래스패스를 지정하여 클래스를 찾음

### import 문
다른 패키지의 클래스를 사용하려면 패키지명이 표함된 클래스 이름을 사용햐야 함  
이 번거로움을 막기위해 import문을 사용 (C++의 #include나 using namespace 같은 개념)

### import문의 선언
- package 문 이전, class 선언 이전에 위치해야함
  - package, import, class
- 한 소스파일에 여러번 선언할 수 있음
```java
import 패키지명.클래스명;
import 패키지명.*;  // 패키지 안에 있는 모든 클래스
```
- 실행 시 성능상 차이는 전혀 없음
- 지금껏 java.lang에 속해있는 System이나 String 같은 클래스를 패키지명 없이 쓸 수 있던 이유는 소스파일에 묵시적으로 import java.lang.*; 이 선언 되어있기 때문임
```java
import java.text.SimpleDateFormat;  // java.text 패키지의 SimpleDateFormat 클래스 사용
import java.util.Date;  //// java.util 패키지의 Date 클래스 사용

class ImportTest {
    public static void main(String[] args) {
        Date today = new Date();

        SimpleDateFormat date = new SimpleDateFormat("yyyy/MM/dd");
        SimpleDateFormat time = new SimpleDateFormat("hh:mm:ss a");

        System.out.println("오늘 날짜는 " + date.format(today));
        System.out.println("현재 시각은 " + time.format(today));
    }
}
```
  
### static import
static 멤버를 호출할 때 클래스 이름을 생략할 수 있음  
```java
import java.lang.System.out;

out.println();  // 클래스 이름 System 생략
```
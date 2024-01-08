# Ch7 객체 지향 프로그래밍 2

## 내부 클래스
클래스 내에서 클래스를 선언하는 것
- 외부 클래스 외에는 잘 사용하지 않는 클래스를 내부 클래스로 선언하면서 코드의 복잡성을 줄일 수 있음 (캡슐화)
  
### 내부 클래스의 종류와 특징
변수와 특징이 비슷함
- 인스턴스 클래스
  - 외부 클래스의 멤버변수 선언 위치에 선언, 외부 클래스의 인스턴스 멤버 처럼 쓰임
- 스태틱 클래스
  - 외부 클래스의 멤버변수 선언 위치에 선언, 외부 클래스의 static 멤버 특히 메서드를 다룰 때 사용
- 지역 클래스
  - 외부 클래스의 메서드나 초기화 블럭 안에 선언
- 익명 클래스
  - 클래스의 선언과 객체의 생성을 동시에 하는 이름 없는 일회용 클래스

### 내부 클래스의 제어자와 접근성
- 내부 클래스도 클래스이기 때문에 abstract, final과 같은 제어자도 사용 가능하고  
- 멤버 변수들 처럼 접근 제어자도 사용 가능하다
- 단, 내부 클래스 중에서 static 클래스만 static 멤버를 가질 수 있다
  - final static 인 경우엔 상수이기 때문에 모든 내부 클래스에서 선언 가능하다

내부 클래스의 접근성 (변수와 유사)

```java
public class InnerEx2 { 
    class InstanceInner {}
    static class StaticInenr {}
    
    // 인스턴스 멤버인 iv가 인스턴스 내부 클래스인 InstanceInner에 접근 -> 가능
    InstanceInner iv = new InstanceInner();
    // 스태틱 멤버인 cv가 스태틱 내부 클래스인 StaticInner에 접근 -> 가능
    static StaticInenr cv = new StaticInenr();
    
    
    static void staticMethod() {
        // static 멤버는 인스턴스 멤버에 직접 접근할 수 없음
        // InstanceInenr obj1 = new InstanceInner();
        StaticInenr obj2 = new StaticInenr();
        
        // 굳이 접근하려면 외부 객체를 생성 하고 접근해야함
        InnerEx2 outer = new InnerEx2();
        InstanceInner obj1 = outer.new InstanceInner();
    }
    
    void instanceMethod() {
        // 인스턴스 메서드에선 인스턴스 멤버 스태틱 멤버 다 접근 가능
        InstanceInner obj1 = new InstanceInner();
        StaticInenr obj2 = new StaticInenr();
    }
    
    void myMethod() {
        int lv = 0;
        final int LV = 0;
        class LocalInner {
            int liv3 = lv;  // 컴파일러가 int lv에 final 삽입
            int liv4 = LV;
        }
    }
}
```
- 기존 인스턴스 멤버와 스태틱 멤버간의 접근성과 같음
- 외부 클래스의 멤버로서 내부 클래스의 객체를 생성할 때 참조변수의 접근성도 신경 써줘야함
- ***지역 클래스가 메서드 내의 지역변수를 사용할 경우 final이 붙은 지역 변수만 접근 가능함***
  - 메서드가 종료 되었을 때 지역 클래스의 인스턴스가 이미 소멸된 지역 변수를 지역변수를 참조하려는 경우가 발생할 수 있기 때문
  - JDK1.8 부터 지역 클래스가 접근하려는 지역 변수 앞에 final을 생략 가능함 그래도 상수이기 때문에 이 값을 변경하려고 하면 컴파일 에러 발생함

### 내부 클래스와 this
```java
class Outer {
    int value = 10;

    class Inner {
        int value = 20;

        void method() {
            int value = 30;
            System.out.println("           value :" + value);
            System.out.println("      this.value :" + this.value);
            System.out.println("Outer.this.value :" + Outer.this.value);
        }
    }
}

class InnerEx5 {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.method();
    }    
}
```
- 내부 클래스 기준
  - value : method1 의 value
  - this.value : Inner 클래스의 value
  - Outer.this.value : Outer 클래스의 value

### 익명 클래스
클래스 선언과 객체 생성을 동시에 하기 때문에 이름이 없고 일회용임
```java
new 조상클래스이름() {
    // 멤버 선언
}

new 구현인터페이스이름() {
    // 멤버 선언
}
```
위와 같이 조상 클래스나 인터페이스의 이름으로 정의하기 때문에 오로지 단 하나의 클래스를 상속 받거나 단 하나의 인터페이스를 구현할 수 있음

#### 익명 클래스로 변환 전
```java
import java.awt.*;
import java.awt.event.*;

class InnerEx7 {
    public static void main(String[] args) {
        Button b = new Button("Start");
        b.addEventListener(new EventHandler());
    }
}

class EventHandler extends ActionListener {
    public void actionPerformed(ActionEvent e) {
        System.out.println("ActionEvent occurred");
    }
}
```

#### 변환 후
```java
import java.awt.*;
import java.awt.event.*;

class InnerEx7 {
    public static void main(String[] args) {
        Button b = new Button("Start");
        b.addEventListener(new ActionListener {
            public void actionPerformed(ActionEvent e) {
                System.out.println("ActionEvent occurred");
            }
        }
    );
    }
}
```
클래스를 별도로 만들지 않고 코드를 간략화 할 수 있다
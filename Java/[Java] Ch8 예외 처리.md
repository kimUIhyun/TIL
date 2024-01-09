# Ch8 예외 처리
## 1. 예외 처리
### 1.1 프로그램 오류
- 컴파일 에러 : 컴파일 시 발생하는 에러
- 런타임 에러 : 실행 시 발생하는 에러
  - 에러 : 메모리 부족, 스택 오버플로우 같이 프로그램 코드에 의해서 수습될 수 없는 오류
  - 예외 : 프로그램 코드에 의해서 수습될 수 있는 가벼운 오류
- 논리적 에러 : 실행은 되지만 의도와 다르게 동작하는 것

### 1.2 예외 클래스의 계층 구조
자바에서는 실행 시 발생할 수 있는 오류를 클래스로 정의함 (Error, Exception)  
그러므로 Error와 Exception 클래스 역시 Object 클래스의 자손들임  

![Throwable Classes](<Throwable Classes.png>)

Exception 클래스

![Exception Classes](<Exception Classes.png>)

예외 클래스는 두 그룹으로 나뉨
- 'checked예외' : Exception 클래스와 그 자손들 (RuntimeException 클래스와 그 자손들 제외)
  - 사용자의  실수 같은 외적인 요인으로 발생하는 예외
  - 발생한 예외에 대해서 컴파일러가 예외처리가 되었는지 확인함
- 'unchecked예외' : RuntimeException 클래스와 그 자손들
  - 프로그래머 실수로 발생하는 예외들 ex) 배열 범위 초과, 형변환, null 호출 등
  - 발생한 예외에 대해서 컴파일러가 예외처리가 되었는지 확인하지 않음

### 1.3 예외 처리하기 - try-catch 문
```java
try {
        // 예외가 발생할 가능성이 있는 문장
}   catch (Exception1 e1) {
        // Exception1이 발생했을 경우, 이를 처리하기 위한 문장
}   catch (Exception2 e2) {
        // Exception2이 발생했을 경우, 이를 처리하기 위한 문장
}   catch (Exception3 e3) {
        // Exception3이 발생했을 경우, 이를 처리하기 위한 문장
}
```
- 여러개를 사용할 수 있고 try-catch 내부에도 사용할 수 있음

```java
class ExceptionEx3 {
    public static void main(String[] args) {
        int number = 100;
        int result = 0;

        for(int i=0; i < 0; i++) {
            try {
                result = number / (int)(Math.random() * 10);    // 정수를 0으로 나누는 예외가 발생할 수 있음
                System.out.println(result);
            } catch (ArithmeticException e) {
                System.out.println("0");
            }
        }
    }
}
```

### 1.4 try-catch문에서의 흐름
- try블럭 내에서 예외가 발생한 경우
  - 발생한 예외와 일치하는 catch 블럭이 있는지 확인
  - 이 때, 예외가 발생한 문장 다음 문장들은 수행되지 않고 바로 catch블럭을 찾음
  - 일치하는 catch 블럭이 있으면 그 안의 문장들을 수행하고 전체 try-catch블럭을 빠져 나와서 다음 문장 수행
- try블럭 내에서 예외가 발생하지 않은 경우
  - catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 계속 수행

### 1.5 예외의 발생과 catch블럭
- 예외가 발생하면 그 예외에 해당하는 예외 클래스의 인스턴스가 만들어짐
- catch블럭의 () 안의 참조변수와 발생한 예외 인스턴스를 instanceof 연산자로 검사 
- true인 catch블럭을 찾으면 블럭의 문장들을 수행
  - 전부 false이면 예외는 처리되지 않음

모든 예외 클래스는 Exception 클래스의 자손이므로 catch블럭 괄호 안에 Exception 클래스 타입의 참조변수를 선언하면 어떤 종류의 예외가 발생하더라도 처리됨

#### printStackTrace()와 getMessage()
- printStackTrace() : 에외발생 당시의 호출스택에 있었던 메서드의 정보와 예외 메시지를 화면에 출력
  - printStackTrace(PrintStream s) 혹은 printStackTrace(PrintWriter s)를 사용하면 발생한 예외에 대한 정보를 파일에 저장할 수도 있음 (Ch15)
- getMessage() : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있음

```java
class ExceptionEx8  {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        try {
            System.out.println(3);
            System.out.println(0/0);
            System.out.println(4);
        } catch (ArithmeticException ae) {
            ae.printStackTrace();
            System.out.println("예외메시지 : " + ae.getMessage());
        }
        System.out.println(6);
    }
}
```
```
1
2
3
java.lang.ArithmeticException: / by zero
      at ExceptionEx8.main(ExceptionEx8.java:7)
예외메시지: / by zero
6
```

#### 멀티 catch블럭
여러 catch 블럭을 '|' 기호를 이용해서 하나의 멀티 catch 블럭으로 힙칠 수 있음
- 연결할 수 있는 예외 클래스의 개수에는 제한이 없음
- 연결하려는 클래스가 조상과 자손의 관계에 있다면 그냥 조상 클래스만 써주는 것과 똑같기 때문에 에러 발생
- 멀티 catch블럭의 참조변수는 상수이므로 변경할 수 없음 (여러 클래스가 하나의 참조변수를 공유하기 때문)

### 1.6 예외 발생시키기
키워드 throw를 사용해서 고의로 예외를 발생시킬 수 있음
- new 연산자로 발생시키려는 예외 클래스의 객체를 만듦
  - Exception e = new Exception("고의로 발생");
  - e.getMessage();는 "고의로 발생"을 반환함
- 키워드 throw를 이용해서 예외를 발생시킴
  - throw e;
- 위의 과정을 한줄로도 가능
  - throw new Exception("고의로 발생");

### 1.7 메서드에 예외 선언하기
메서드의 선언부에 throws를 사용하여 메서드 내에서 발생할 수 있는 예외를 적어 줄 수 있음
```java
void method() throws Exception1, Excption2, ... Exception N {

}
```
- 메서드를 사용하는 쪽에선 이에 대한 처리를 해주어야함
- unchecked 예외는 반드시 처리하지 않아도 되므로 적어주지 않아도 됨

```java
class ExceptionEx14 {
    public static void main(String[] args) {
        try {
            method1();  // 함수 호출문을 에외 문장으로
        } catch (Exception e) {
            System.out.println("main에서 예외 처리");
            e.printStackTrace();
        }
    }
    
    static void method1() throws Exception {
        throw new Exception();
    }
}
```
- method1()에서 발생한 예외를 main에서 try-catch로 처리
- method1()에서 try-catch로 처리할 수도 있음
- 분담할 수도 있음

### 1.8 finally 블럭
예외의 발생여부에 상관 없이 실행 되어야 할 코드를 포함. try-catch-finally의 순서로 구성됨
- try나 catch문에서 return문이 실행되는 경우에도 finally 블럭이 먼저 실행된 후에 return 문이 실행됨

### 1.9 자동 자원 반환 try-with-resources 문
입출력과 관련된 클래스를 사용할 때 유용함 (Ch15, 추후 추가 예정)
- close()에서 예외가 발생했을 때 finally안에서 또 예외 처리를 해줘야 하는데 이 때 기존의 try블럭과 finally블럭에서 예외가 발생하면 try블럭은 무시됨. 이를 방지하기 위해 close()를 더 쉽게 예외처리 하기 위함
```java
try (FileInputStream fis = new FileInputStream("score.dat"); 
    DataInputStream dis = new DataInputStream(fis)) {

    while (true) {
        score = dis.readInt();
        System.out.println(score);
        sum += score;
    }
}   catch (EOFException e) {
    System.out.println("점수의 총합은 " + sum +"입니다.");
}   catch (IOException ie) {
    ie.printStackTrace();
}
```
- try의 ()안에 객체를 생성하는 문장을 넣으면 이 객체는 try를 벗어나는 순간 자동적으로 close()가 호출 됨. 그 다음에 catch, finally 수행

### 1.10 사용자 정의 예외 만들기
```java
class MyException extends Exception {
    MyException(String msg) { super(msg); } // 메시지 받는 생성자
}
```
- 기존 정의된 예외 클래스 외에 새로운 예외 클래스를 정의하여 사용할 수 있음
- 보통 Exception 클래스 또는 RuntimeException 클래스로부터 상속 받아 만들고 필요에 따라 알맞은 예외 클래스를 선택할 수 있음
- 필요하다면 멤버를 선언할 수 있음

### 1.11 예외 되던지기 re-throwing
메서드에서 try-catch 문으로 예외를 처리하고 catch 블럭에서 예외를 또 발생시켜 호출한 메서드에서도 예외처리를 하도록 함
- 메서드 내부적으로 발생할 수 있는 예외를 처리하고 메서드를 호출하여 사용할 때 발생할 수 있는 예외를 또 처리하도록 할 때
```java
class ExceptionEx17 {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            System.out.println("main exception");
        }
    }

    static int method1() throws Exception {
        try {
            throw new Exception();
            return 0;
        } catch (Exception e) {
            System.out.println("method1 exception");
            return 1;   // 반환값이 있는 메서드인 경우 catch 블럭 까지 return 문이 있어야함
            throw e;    // re-throwing
        }
    }
}
```

### 1.12 연결된 예외
한 예외가 다른 예외를 발생시킬 수도 있음 (원인 예외 cause exception)
```java
catch (SpaceException e) {
    InstallException ie = new InstallException();
    ie.initCause(e);    // InstallException의 원인 예외를 SpaceException으로 지정
    throw ie;
}
```
- initCause()는 Exception 클래스의 조상인 Throwable 클래스에 정의되어 있기 때문에 모든 예외에서 사용 가능
- 두 예외를 한번에 처리할 때 상속 관계로 해서 처리하면 어떤 예외가 발생하였는지 알기 힘들기 때문에 상속 관계로 묶지 않고도 엮을 수 있도록 함
- checked 예외를 unchecked 예외로 바꿀 수 있음

checked 예외 MemoryException
```java
static void startInstall() throws SpaceException, MemoryException {
    if(!enoughSpace())
        throw new SpaceException();
    if(!enoughMemory())
        throw new MemoryException();
}
```
unchecked 예외
```java
static void startInstall() throws SpaceException {
    if(!enoughSpace())
        throw new SpaceException();
    if(!enoughMemory())
        throw new RuntimeException(new MemoryException());  // RuntimeException 의 원인 예외를 등록하는 생성자
}
```

### 예제
```java
class ChainExceptionEx {
    public static void main(String[] args) {
        try {
            install();
        } catch(InstallException e) {
            e.printStackTrace();
        } catch(Exception e) {
            e.printStackTrace();
        }
    }

    static void install() throws InstallException { // 매서드 예외
        try {
            startInstall(); // 예외 발생
            copyFiles();
        } catch(SpaceException se) {
            InstallException ie = new InstallException("설치중 예외 발생");
            ie.initCause(se);   // 연결된 예외
            throw ie;
        } catch(MemoryException me) {
            InstallException ie = new InstallException("설치중 예외 발생");
            ie.initCause(me);   // 연결된 예외
            throw ie;
        } finally {
            deleteTempFiles();
        }
    }

    static void startInstall() throws SpaceException, MemoryException { // 메서드 예외
        if(!enoughSpace()) {
            throw new SpaceException("설치할 공간이 부족합니다.");
        }
        if(!enoughMemory()) {
            throw new MemoryException("메모리가 부족합니다.");
        }
    }

    static void copyFiles() {}
    static void deleteTempFiles() {}

    static boolean enoughSpace() {
        return false;
    }
    static boolean enoughMemory() {
        return true;
    }
}

class InstallException extends Exception {  // 사용자 지정 예외
    InstallException(String msg) {
        super(msg);
    }
}

class SpaceException extends Exception {    // 사용자 지정 예외
    SpaceException(String msg) {
        super(msg);
    }
}

class MemoryException extends Exception {   // 사용자 지정 예외
    MemoryException(String msg) {
        super(msg);
    }
}
```
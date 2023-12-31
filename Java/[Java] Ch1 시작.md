# Ch1 시작

### 자바란?
*자바 == 표준 스펙 + 구현*
- 자바 표준 스펙
  - 자바는 이렇게 만들어야 한다는 설계도, 스펙
  - 이 표준 스펙을 기반으로 여러 회사에서 실제 돌아가는 자바를 만듦
  - 자바 표준 스펙은 자바 커뮤니티 프로세스(JCP)를 통해 관리
- 자바 구현
  - 여러 회사에서 자파 표준 스펙에 맞춰 제작
  - 각각 특징과 장단점이 있음
  - 그래도 표준 스펙에 맞춰서 제작 되었기 때문에 전환 해도 문제는 없음
  - 다양한 자바 구현은 다음 참조 http://whichjdk.com/ko


### 컴파일과 실행
컴파일과 실행 과정
1. Hello.java 와 같은 자바 소스 코드를 개발자가 작성
2. 자바 컴파일러를 사용해서 소스 코드를 컴파일
    - 자바가 제공하는 javac라는 프로그램을 사용
    - .java -> .class 로 컴파일
    - 자바 소스 코드를 바이트 코드로 변환하며 JVM에서 빠르게 실행 되도록 최적화 하고 문법 오류 검출
3. 자바 프로그램 실행
    - 자바가 제공하는 java라는 프로그램 사용
    - 자바 가상 머신(JVM)이 실행 되면서 프로그램 작동

### IDE와 자바
인텔리제이를 통한 자바 설치 관리 가능
- 직접 os에 설치 해도 되지만 편리한 맛에 사용

### 인텔리제이를 통한 자바 컴파일, 실행
- 컴파일
  - javac 프로그램을 인텔리제이에서 실행해줌
  - 프로젝트의 out디렉토리 보면 .class 파일들 생성 되어있음
- 실행
  - java 라는 프로그램을 사용해 컴파일 된 .class 파일을 실행 시켜야함
  - java Hello (확장자는 제외함)
  - 인텔리제이에서 알아서 한번에 해결함
  
- 인텔리제이는 컴파일러 + 자바 실행기


### 자바와 운영체제의 독립성
*자바는 자바가 설치된 모든 OS에서 실행 가능함*
- 컴파일된 코드가 각각 OS에 설치된 java에서 알아서 각자의 명령어로 실행 됨
- 자바 개발자는 특정 OS에 맞추어 개발하지 않아도 됨
- 개발자는 맥OS를 사용하고 서버는 주로 사용하는 AWS 리눅스 서버를 사용한다 하면 그냥 각자의 OS에 맞는 java를 설치해 두고 개발, 배포 하면 됨

### 자바의 특징
- 운영체제에 독립적
- 객체 지향 언어 (OOP)
- 자동 메모리 관리
  - 사용하지 않는 메모리를 자동으로 체크하고 반환해줌
- 네트워크와 분산처리 지원
- 멀티쓰레드
- 동적 로딩
  - 실행시에 모든 클래스가 로딩 되지 않고 필요한 시점에 동적으로 로딩됨
  - 그러므로 일부 클래스가 변해도 전체 어플리케이션을 다시 컴파일 하지 않아도 됨 


## HelloJava.java
```java 
public class HelloJava {

    public static void main(String[] args) {
        System.out.println("hello java");
    }
}
```
- HelloJava.java -> 컴파일(javac) -> HelloJava.class -> 실행(java, JVM) -> java HelloJava
- 코드는 클래스 안에 존재함
- 자바 문법은 대소문자를 구분함
- 소스 파일의 이름은 public class 의 이름과 같아야함
- 한 클래스 안에 하나의 public class 만이 존재 가능
- psvm으로 public static void main(String[] args) 한번에 입력 가능


## Comment Java
```java
public class CommentJava {
    public static void main(String[] args) {
        System.out.println("hello java1"); //주석1
        //System.out.println("hello java2");

        /*
        System.out.println("hello java3");
        System.out.println("hello java4");
         */
    }
}
```

- 주석은 c++에서와 동일함
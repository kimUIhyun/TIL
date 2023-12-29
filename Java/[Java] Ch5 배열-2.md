# Ch5 배열-2

## String 배열
String은 클래스이므로 String배열은 각각의 String객체를 가리키는 주소들을 가진 참조형(객체) 배열이다
- string 클래스는 char 배열에 여러 기능을 추가한 것임
- 기본적인 기능(매서드)
  - char charAt(int index) : 문자열에서 해당 위치에 있는 문자 반환
  - int length() : 문자열의 길이 반환
  - String substring(int from, int to) : 문자열에서 해당 범위에 있는 문자열을 반환 [from, to)
  - boolean equals(Object obj) : 문자열의 내용이 obj와 같은지 확인. 대소문자 구분함 (equalsIgnoreCase는 구분 안함)
  - char[] toCharArray() : 문자열을 문자배열(char[])로 변환해서 반환한다
    - char[] tmp = str.toCharArray();
    - String str = new String(chaArr) : char배열 -> String
```java
public class ArrayEx15 {
    public static void main(String[] args) {
        String source = "SOSHELP";
        String[] morse = {".-", "-...", "-.-.", "-,,", "."
                        ,"..-.", "--.", "....", "..", ".---"
                        ,"-.-", ".-..", "--", "-.", "---"
                        ,".--", "--.-", ".-.", "...", "-"
                        ,"..-", "...-", ".--", "-..-", "-.--"
                        ,"--.." };
        String result = "";

        for (int i = 0; i < source.length(); i++) {
            result += morse[source.charAt(i) - 'A'];
        }
        System.out.println("source:"+ source);
        System.out.println("morse:"+ result);
    }
}
```
- 문자열 source에서 String 클래스의 char charAt(int index) 메서드와 유니코드 연산을 이용해 문자열 배열 morse의 원소에 접근

## 커맨드 라인 통해 입력 받기
JVM이 프로그램을 실행할 때 커맨드 라인을 통해 java Ex abc 123 이런 식으로 Ex 클래스 안의 main(String args[]) 메서드의 매개변수 args[] 의 인자로 전달 할 수 있다
- 이 때 공백으로 문자열을 구분하기 때문에 문자열에 공백이 있을 경우엔 " "로 감싸 줘야함
- 커맨드 라인에 매개변수를 입력하지 않으면 JVM이 크기가 0인 args[]를 생성함
  - 배열을 아예 생성하지 않을 경우 참조 변수 args의 값이 null이 되기 때문에 args를 사용하는 문장에서 오류 발생함, 크기가 0인 배열을 생성하여 이를 방지함
```java
public class ArrayEx17 {
    public static void main(String[] args) {
        if (args.length != 3) {
            System.out.println("usage: java ArrayEx17 NUM1 OP NUM2");
            System.exit(0); // 프로그램 종료
        }
        int num1 = Integer.parseInt(args[0]);
        char op = args[1].charAt(0);
        int num2 = Integer.parseInt(args[2]);
        int result = 0;

        switch(op) {
            case '+':
                result = num1 + num2;
                break;
            case '-':
                result = num1 - num2;
                break;
            case '*':
                result = num1 * num2;
                break;
            case '/':
                result = num1 / num2;
                break;
            default:
                System.out.println("this operation is not supported.");
        }
        System.out.println("결과: " + result);
    }
}
```
- 커멘드 라인에서 java ArrayEx17 실행시 뒤에 문자열 3개를 포함 시켜 실행시킴

## 다차원 배열
다차원 배열은 배열의 배열임
- 2차원 배열 arr[5][3]의 경우 참조변수 arr은 배열의 배열인 arr[0], arr[1],.. 을 가리키게 되게 각각의 배열은 원소들 arr[0][1], arr[0][2], arr[0][3], arr[1][0], arr[1][1]... 을 가짐
- arr.length 의 경우 arr은 배열의 배열을 가리키기 때문에 5이고 각 배열의 길이 예를 들어 arr[0].length 의 경우 3이다
- 마지막 차수의 배열 크기는 정해두지 않고 가변적으로 할당해줄 수 있음
    - ex) arr[0] = new int[3];&nbsp;&nbsp; arr[1] = new int[5];&nbsp;&nbsp; arr[2] = new int[0];&nbsp;...
    - 이 형태로 생성과 동시에 초기화도 가능
  

#### 2차원 배열에서 향상된 for문의 사용
```java
public class ArrayEx18 {
    public static void main(String[] args) {
        int[][] score = {
                            { 100, 100, 100}
                            , { 20, 20, 20}
                            , { 30, 30, 30}
                            , { 40, 40, 40}
                        };
        int sum = 0;
        
        for (int[] tmp : score) { // 2차원 배열에서 향상된 for문의 사용
            for (int i : tmp) {
                sum += i;
            }
        }
        System.out.println("sum="+sum);
    }
}
```
- score가 가리키는 각 요소들은 1차원 배열이므로 이 배열들을 int[] tmp로 하나씩 읽어들이고 각 1차원 배열인 tmp를 i로 다시 읽어들인다

### 다차원 배열 예제
```java
import java.util.*;

public class MultiArrEx1 {
    public static void main(String[] args) {
        final int SIZE = 10;
        int x = 0, y =0;

        char[][] board = new char[SIZE][SIZE];
        byte[][] shipBoard = {
                {0, 0, 0, 0, 0, 0, 0, 1, 0},
                {1, 1, 1 ,1, 0, 0, 1, 0, 0},
                {0, 0, 0, 0, 0, 0, 1, 0, 0},
                {0, 0, 0, 0, 0, 0, 1, 0, 0},
                {0, 0, 0, 0, 0, 0, 0, 0, 0},
                {1, 1, 0, 1, 0, 0, 0, 0, 0},
                {0, 0, 0, 1, 0, 0, 0, 0, 0},
                {0, 0, 0, 1, 0, 0, 0, 0, 0},
                {0, 0, 0, 0, 0, 1, 1, 1, 0}
        };

        for (int i =1; i<SIZE; i++)
            board[0][i] = board[i][0] = (char)(i+'0');

        Scanner scanner = new Scanner(System.in);

        while(true) {
            System.out.printf("좌표를 입력하세요. (종료는 00)>");
            String input = scanner.nextLine();

            if(input.length() == 2) {
                x = input.charAt(0) - '0';
                y = input.charAt(1) - '0';

                if(x==0 && y==0)
                    break;
            }
            if(input.length() != 2 || x<=0 || x>=SIZE || y<=0 || y>=SIZE) {
                System.out.println("잘못된 입력입니다. 다시 입력해주세요.");
                continue;
            }

            board[x][y] = shipBoard[x-1][y-1] == 1 ? 'O' : 'X'; // 조건 연산자 사용

            for(char[] tmp : board) // 향상된 for문
                System.out.println(tmp); // board가 char 배열이기 때문에 참조변수만 전달해도 배열 전체가 출력 됨
            System.out.println();
        }
    }
}
```
- char 2차원 배열의 출력시 배열의 배열만 출력해도 char 배열이기 때문에 원소 전체가 출력됨


# Ch4 조건문과 반복문

## 조건문
### 가위 바위 보
```java
import java.util.*;

public class FlowEx7 {
    public static void main(String[] args) {
        System.out.print("가위(1), 바위(2), 보(3) 중 하나를 입력하세요.");

        Scanner scanner = new Scanner(System.in);
        int user = scanner.nextInt();
        int com = (int)(Math.random() * 3) + 1; // 1,2,3중 하나가 com에 저장

        System.out.println("당신은 " + user +"입니다.");
        System.out.println("컴퓨터는 " + com +"입니다.");

        switch(user - com) {
            case 2: case -1:
                System.out.println("당신이 졌습니다.");
                break;
            case 1: case -2:
                System.out.println("당신이 이겼습니다.");
                break;
            case 0:;
                System.out.println("비겼습니다.");
                break;
        }

    }
}
```
- Math.random() : 0.0 <= d < 1.0 인 하나의 double 값을 반환
  -  1부터 3까지의 난수를 생성하기위해 *3 하고 정수로 형변환 후 +1

### 주민번호 읽기
```java
import java.util.*;

public class FlowEx8 {
    public static void main(String[] args) {
        System.out.print("주민번호를 입력하세요. (112233-1111222)>");

        Scanner scanner = new Scanner(System.in);
        String regNo = scanner.nextLine();

        char gender = regNo.charAt(7);

        switch(gender) {
            case '1': case '3':
                System.out.println("남자입니다.");
                break;
            case '2': case '4':
                System.out.println("여자입니다.");
                break;
            default:
                System.out.println("유효하지 않은 주민번호입니다.");
        }
    }
}
```
- regNo.charAt(n) : 문자열 regNo의 n번째 자리의 문자를 char로 읽어들임
    - char로 읽어왔기 때문에 switch문에서 정수가 아닌 유니코드 문자로 케이스를 나눔

## 반복문

### for
```java
for(타입 변수명 : 배열 또는 컬렉션) {
    //반복문
}
```
- JDK1.5 부터 배열과 컬렉션에 편하게 접근하도록 추가됨
- 오른쪽에있는 배열 또는 컬렉션에 저장된 값이 매 반복마다 하나씩 순서대로 왼쪽 변수에 저장됨
```java
int[] arr = {1,2,3,4,5};

for(int tmp : arr) {
    System.out.print("%d ", tmp);
}
```
```
1 2 3 4 5
```

### 반복문 이름 지정
```java
public class FlowEx33 {
    public static void main(String[] args) {
        Loop1 : for(int i=2; i <= 9; i++) { // 반복문에 이름 지정
            for(int j=1;j <=9; j++) {
                if(j==5)
                    break Loop1; // 맨앞 for문 break
                System.out.println(i+"*"+ j +"="+ i*j);
            }
            System.out.println();
        }
    }
}
```
- 반복문에 이름 지정 가능, break와 continue를 지정해서 사용 가능



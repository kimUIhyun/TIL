# Ch5 배열

### 배열의 생성
- 배열의 선언
  - 타입[] 변수이름 &nbsp;&nbsp;&nbsp;int[] array;
  - 타입 변수이름[] &nbsp;&nbsp;&nbsp;int array[];
- 배열의 생성
  - 변수 이름 = new 타입[길이] &nbsp;&nbsp;(길이 >= 0)
  - int[] array = new int[5];
  - 참조변수 array에는 주소가 아니라 타입@주소 가 저장됨
    - ex) I@14318bb 
- int형 참조변수 array 생성 -> 연산자 new로 인해 메모리에 int형 공간 5개 생성 -> 각각 0으로 초기화 -> 대입 연산자 =에 의해 배열의 주소가 참조변수 array에 저장

### 배열의 길이
- 배열의 길이는 JVM에서 (배열이름).length 로 별도의 상수로서 관리 된다
  - int tmp = array.length; // tmp = 5
  - 상수이기 때문에 변경 안됨
  - 배열의 길이를 변경한 경우 수정이 편하고 에러 발생 확률이 적기 때문에 반복문 같은 곳에서 사용 권장

### 배열의 초기화
- 반목문으로 대입해서 초기화
- 생성과 동시에 초기화
  - int[] array = new int[] { 1, 2, 3, 4, 5};
    - 초기화 하면서 길이 자동으로 정해짐
    - int[] array = { 1, 2, 3, 4, 5 }; &nbsp;&nbsp; new int[] 생략 가능
    - 선언과 생성을 따로하는 경우에는 생략 불가능
    - 매개변수로 배열을 받는 메서드를 호출할 때도 생략 불가능

### 배열의 출력
- 반복문으로 출력
- Arrays.toString(array) 메서드 사용
    - 배열의 모든 요소를 [첫번째, 두번째, ...] 형식의 문자열로 만들어서 반환함
    - import java.util.Array 추가해야함
    - System.out.println(Arrays.toString(array));
- char형 배열인 경우에는 println메서드가 배열의 참조변수만 받아도 배열의 요소를 출력함

### 배열의 복사
- for(int i = 0; i < num.length; i++) { newNum[i] = num[i]; }
- System.arraycopy(num, 0, newNum, 0, nem.length);
  - num[0]에서 newNum[0]으로 num.length개의 데이터 복사

### 배열 길이 변경
```java
int[] arr = {1,2,3,4,5};
int [] tmp = new int[arr.length*2];
System.arraycopy(arr, 0, tmp, 0, arr.length);
arr = tmp;
```
- 2배 크기 배열 생성후 복사하고 참조값 변경
- 기존 참조변수를 잃어버린 arr 배열의 메모리 공간들은 JVM에 의해서 자동으로 반환

### 예제
- 배열 원소 섞기
    - 난수 발생으로 무작위 인덱스 선택해 [0]과 교환
```java
import java.util.Arrays;

public class ArrayEx7 {
    public static void main(String[] args) {
        int[] numArr = {0,1,2,3,4,5,6,7,8,9};
        System.out.println(Arrays.toString(numArr));

        for (int i=0; i < numArr.length; i++) {
            int n = (int)(Math.random() * 10);
            int tmp = numArr[0];
            numArr[0] = numArr[n];
            numArr[n] = tmp;
        }
        System.out.println(Arrays.toString(numArr));
    }
}
```

- 버블 정렬
```java
public class ArrayEx10 {
    public static void main(String[] args) {
        int[] numArr = new int[10];

        for (int i=0; i < numArr.length; i++) {
            System.out.print(numArr[i] = (int)(Math.random() * 10));
        }
        System.out.println();

        for (int i=0; i < numArr.length - 1; i++) {
            boolean changed = false;

            for (int j=0; j < numArr.length-1-i; j++) {
                if(numArr[j] > numArr[j+1]) {
                    int tmp = numArr[j];
                    numArr[j] = numArr[j+1];
                    numArr[j+1] = tmp;
                    changed = true;
                }
            }

            if (!changed) break;

            for(int k : numArr) { // 향상된 for문
                System.out.print(k);
            System.out.println();
        }
    }
}
```
## String 배열
String은 클래스이므로 String배열은 각각의 String객체를 가리키는 주소들을 가진 참조형(객체) 배열이다
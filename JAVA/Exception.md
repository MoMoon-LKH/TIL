# 예외 처리
## 예외
    예상 및 예상 못한 것에 대한 것에 대해 미연에 방지

### 1) try-catch-finally
```java
public void sameple{
    int[] intArray = new int[3]; // try내에 선언하면 catch문에서 사용 못함
    
    try{
        // 문제가 생길만한 부분
    } catch(NullPointerException e) {
        // null 예외 처리
    } catch (Exception e) {
        // 예외 발생했을 때 처리할 부분
    } finally {
        // 무조건 마지막에 실행될 부분
    }
    
}
```
### (1) try
- 예외가 발생할만한 블럭

### (2) catch
- 예외 발생했을 때 처리하는 블럭
- 여러개 선언가능 ( 순서 중요 )

### (3) finally
- 어떠한 경우에도 무조건 실행하는 구문
- 코드 중복을 피하기위한 블럭

<br>

----------------------------------------

## 2) 예외 종류
### (1) error
- 자바 프로그램 밖에서 발생한 예외
- 외부의 요인으로 인해 발생한 에러

### (2) checked exception
- 컴파일 시 발생하는 에러

### (3) unchecked exception 혹은 runtime exception
- 실행 중에 예외가 발생하는 예외
- 컴파일 시 체크를 안함


<br>

---------------------------------

## 3) 예외 클래스 java.lang.Throwable
    Exception과 Error의 공통 부모 클래스

### 예외 메소드
### (1) getMessage()
- String 형태로 예외 리턴
- 간단한 예외 메시지

### (2) toString()
- String 형태로 예외 리턴
- getMessage() 메소드보단 더 자세함


### (3) printStackTrace()
- 예외 메시지 출력 및 예외 발생 메소드들의 호출 관계를 출력
- 출력 메시지가 많아서 개발 시에만 사용하는게 좋음

<br>

------------------------------------

## 4) throws
```java
public void throwException(int num) throws RuntimeException {
    if(num > 12) {
        throw new Exception("exception");    
    }
    
    System.out.println("number is " + num);
}
        
```
- 해당 선언 메소드 호출 시 try-catch문으로 처리

<br>

---------------------------------------

## 5) 예외 임의 생성
```java
public class MyException extends RuntimeException { // Exception 클래스 상속
    
    public MyException() {
        super();
    }
    
    public MyException(String message) {
        super(message);
    }
}
```
- 실행 시 발생하는 예외 경우 Checked 예외가 포함된 Exception 보단 RuntimeException을 상속받는게 좋다

<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)

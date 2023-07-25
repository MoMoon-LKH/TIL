# 인터페이스와 abstract Class
## 1. 인터페이스 ( Interface )
```java
public interface MemberManager {
    public void addMember(MemberDto memberDto);
    ...
}
```
#### - 구현된 메소드가 없이 선언만 있는 설계도

### 1) 인터페이스가 왜 필요한가?
 - 설계단계에서 인터페이스로 변수와 메소드를 선언해두면 개발 시 구현부만 만들면 된다 <br>
    ㄴ 효율적으로 관리가 가능하다
 - 개발자의 취향에 따른 메소드의 이름과 변수 선언에 대한 격차가 줄일 수 있다
 

### 2) 인터페이스 구현 클래스
```java
public class MemberManagerImpl implements MemberManager { // interface를 추가
    
    @Override
    public void addMember(MemberDto memberDto) {
        ...
    }
    ...
}
```
- Override를 통해 메소드를 구현
- 여러개의 인터페이스를 구현할 수 있다

<br>

-------------------------------------------------

## 2. abstract Class
```java
public abstract class MemberManager {
    public abstract boolean addMember(MemberDto member); // 선언 메소드
    
    public void printMember(Member member) { // 구현 메소드
        ...
    }
}
```
- 추상 클래스
- 인터페이스와 다르게 구현이 아닌 상속한다
- interface처럼 메소드를 선언만 할 수도 있고 구현도 가능하다

### 1) 사용 이유
- 인터페이스의 메소드 선언과 더불어 공통적으로 구현해도 문제가 없는 메소드가 있을 때 사용

### 2) abstract 구현 클래스
```java
public class MemberManagerImpl extends MemberManager { // abstract 클래스 상속
    ...
}
```

<br>

-------------------------------------------------

## 3. fianl
### 1) Class
```java
public final class FinalClass {
    ...
}

```
- 자식 클래스에게 상속해줄 수 없음

### 2) Method
```java
public class FinalClass {
    public final void findMethod(){ 
        ...
    }
}
```
- 자식 클래스에서 오버라이딩이 불가능

### 3) Variable
```java
public class FinalClass {
    final int instanceValue = 1;
    
    public void method(final int param) {
        final int localVariable;
    }
}
```
- 클래스 및 인스턴스 변수 같은 경우는 초기화를 시켜줘야 된다
- 매개변수나 지역변수는 초기화하지않아도 되지만 재할당은 불가능

--------------------------------------

## 4. enum
```java
public enum Month {
    JANUARY,
    FEBRUARY,
    .... ,
    DECEMBER;
}
```
- 열거형 클래스
- 상수 (고정된 값)의 집합 클래스

### 1) 상수 값 지정
```java
public enum OverTime() {
    ONE_HOUR(9000), 
    ...

    WEEKEND_EIGHT_HOUR(60000);

    private final int amount;
    
    OverTime(int amount) { // 생성자 지정 가능
        this.amount = amount;
    }
    
    public int getAmount(){
        return amount;
    }
}

```
- 생성자를 만들 수 있지만 package-private와 private의 접근제어자만 가능
    

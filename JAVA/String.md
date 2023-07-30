# String
## 클래스 선언 
```java
public final class String extends Ojbect
    implements Serializable, Comparable<String>, CharSequence
```
- final로 선언되어 상속을 하지 못한다

### 2. 인터페이스
### 1) Serializable
- 해당 객체를 파일로 저잦ㅇ하거나 다른 서버에 전송 가능한 상태가 됨
- 구현할 메소드가 없는 인터페이스

### 2) Comparable
- 매개 변수로 넘어가는 객체와 현재 객체가 같은지 비교, 보통 객체 순서 처리할 때 유용
- 같다면 0, 순서상 앞에 있다면 -1, 뒤에 있다면 1를 리턴
- compareTo() 하나만 선언되어있는 메소드

### 3) CharSequence
- 문자열을 다루기 위한 클래스라는 것을 명시적으로 나타냄
- StringBuilder, StringBuffer 클래스는 해당 인터페이스에 구현되어있음
<br>

## 특징
### 1) 기존의 동일한 값이 있다면 해당 객체를 재사용
```java
public boolean checkEquals() {
    String text = "value";
    String text2 = "value";
    
    if(text == text2) { // 동일 객체인지 비교
        return true;        
    }
    
    return false;
}

// true
```
Constant Pool를 통해 동일한 값을 갖는 객체가 있다면 해당 객체를 재사용한다

<br>

```java
public boolean checkEquals() {
    String text = "value";
    String text2 = new String("value");

    if(text == text2) { // 동일 객체인지 비교
        return true;
    }

    return false;
}

// false
```
### 하지만 생성자를 통해 생성하게되면 값을 재활용 하지않고 별도의 객체로 생성한다 

<br>

### 2) String 문자열을 더하면 다른 객체 생성
- String은 더하기 연산밖에 없다
```java
String text = "String";
text = text + " test";
```
위 처럼 String 문자열을 더하게되면 기존의 객체는 버려지고 새로운 String 객체로 생성된다
- 버려진 객체는 가비지 콜렉션(GC, Garbage Collection) 의 대상이 된다
- 이를 보완하고자 StringBuffer, StringBuilder 클래스가 있다

<br>

## StringBuffer와 StringBuilder
- String 통해 더할 때 새로운 객체가 생성되지 않도록 보완한 Class들
- for문으로 String으로 더할 경우 해당 클래스로 변환해주지않음
- 
### StringBuffer와 SpringBuilder의 차이
StringBuffer의 경우 Thread safe <br>
StringBuilder의 경우 Thread safe X <br>
여러 쓰레드를 통해 해당 인스턴스 변수에 접근할 경우가 있다면 StringBuffer 써야된다
# 객체

------------------------

## 1. 객체란?
- 각각의 실제 사물을 나타내기 위한 것
- 참조 자료형
```java
public class Car{
    
    int speed;
    
    int distance;
    ...
    
    public void speedUp() {
        speed++;    
    }
    
    public static void main(String[] args) {
        Car car = new Car(); // 생성자를 통해 객체 생성
        
        car.speedUp();
    }
}
```
### 클래스와 객체의 다른점
>클래스의 생성자를 통해 객체를 생성 <br>
>즉, 클래스는 설계도고 객체는 설계도를 통해 생성된 각각의 사물
<br>

## 2. 클래스와 메소드
### 1) 클래스
    상태와 행위를 가지고 있는 자바의 가장 작은 단위

```java
public class Printer {
    
    // 상태
    int amountOfInk;
    
    // 행위
    public String print(String content) {
        // 메서드
    }
}
```

### 2) 메소드
- 클래스의 행위에 해당되며 타입과 이름을 가진 매개변수를 받고 리턴타입에 해당되는 결과값을 반환해주는 함수

#### (1) 구조
```java
public boolean checkPassword(String password) {
접근제어자 ㄴ리턴타입 메소드 이름    매개변수
    // 메소드 내용
}

```
> ### 메소드는 반드시 클래스 안에서 정의되어야 한다
<br>



## 3. 생성자
```java
public class Example() {
    
    int num;
    
    public example() {}
    
    public example(int num) {
        this.num = num;
    }
    
    public static void main(String[] args) {
        Example example = new Example();
        Example exampleNum = new Example(2);
    }
}
```
### 특징
- 생성하지않아도 자동으로 매개변수 없는 기본 생성자가 있다
- 만약 다른 생성자를 생성하였다면 기본 생성자는 만들어지지 않는다
- 개수 제한 없음

### this 예약어
    해당 객체를 말함
    - 생성자와 메소드 안에서 사용가능
<br>

## 4. static
### 1) static 메소드
```java

public class Car {
    static String name = "car";
    ...
    
    public static String getCarName() {
        return name;
    }
    
    public static void main(String[] args) {
        
        String name = Car.getCarName(); // 객체를 생성하지않아도 호출 가능
    }
}
```
> static 메소드는 클래스 변수 (static 변수)만 사용가능하다

### 2) static 블럭
- 객체가 생성되기 전에 한번만 호출되는 메소드

```java
public class Block {
    static int data = 1;
    
    public Block() {
        System.out.println("constructor");
    }
    
    static {
        data = 3;
        System.out.println("First");
    }
    
    static {
        data = 5;
        System.out.println("Second");
    }
    //static 블럭은 절차대로 작동

    public static void main(String[] args) {
        Block block1 = new Block();
        Block block2 = new Block();
    }      
    
    /**
     결과
     First
     Second
     Constructor
     Constructor
     **/
}

```

<br>

## 5. Pass by Value, Pass by Reference
### 1) Pass by Value
#### 값만 전달
- 기본 자료형
- 기존의 값이 변하지 않음

```java
public class PassByValue {

    public void passValue(int b) {
        b = 2;
        System.out.println("pass " + 2);
    }
    
    public static void main(String[] args) {
        PassByValue passByValue = new PassByValue();
        
        int a = 1;
        System.out.println(a);
        
        passByValue.passValue(a);
        
        System.out.println(a);
    }
    
    /**
        결과
        1
        2
        1
     */
}

```
> ### 전달한 변수의 값은 변함이 없다
<br>

### 2) Pass by Reference
#### 주소 값을 전달
- 참조 자료형
- 참조형 객체를 넘길 경우 안에 데이터가 변경 시 해당 값 유지
```java
public class PassByReference {
    
    public static void main(String[] args) {
        PassByReference reference = new PassByReference();
        MemberDto memberDto = new MemberDto("kim");
        
        System.out.println(memberDto.name);
        reference.passReference(memberDto);
        System.out.println(memberDto.name);
    }
    
    public void passReference(MemberDto memberDto) {
        memberDto.name = "lee";
        System.out.println(memberDto.name);
    }

    /**
         결과
         kim
         lee
         lee
     */
}

```
> ### 주소값을 넘겼기 때문에 객체의 상태를 변경하면 해당 상태가 유지된다
<br>

### 3) 자바는 모든 동작이 Pass by Value이다
- 주소을 복사해서 해당 주소를 값으로 넘겨준다
> ### 자바에서는 주소로 바로 넘겨주는게 아닌 해당 주소를 복사하여 값으로 넘겨주기 때문에 자바는 PassByValue이다


## 6. 오버로딩 ( Overloading )
- 메소드 이름, 접근제어자, 리턴타입은 같지만 매개변수의 종류와 개수, 순서가 다르면 다른 메소드로 인식하는 기능

```java

public void example(int num) {
    ...
}

public void example(String name) {
    ...
}

public void example(int num1, int num2) {
    ...
}

```
<br>

### DTO와 VO
#### DTO (Data Transfer Object)
- 데이터를 다른 서로 전달하기 위한 것이 주목적인 객체

#### VO(Value Object)
- 데이터를 담아두기 위한 목적

> #### DTO > VO, DTO가 VO보다 하는 일이 많기 때문에 보통 DTO를 선호

<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)

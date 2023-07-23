# 상속
## 상속은 확장의 개념이다
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/fe39a1d7-25f2-41c4-82ed-c89574fbbb05" width="200">

```java
public class Parent {
    ...
}

public class Child extends Parent { // Parent를 상속 받음
    ...
}
```
- 부모 클래스의 선언되어 있는 모든 변수와 메소드를 사용이 가능하다 ( 접근제어자 주의 )
- 다중 상속 X 
### 공통되는 부분을 부모 클래스로 통합하여 상속받은 클래스들에서 사용이 가능하다

<br>

### 1) 부모 생성자 super()
```java
public class Parent {
    
    public Parent(String name) {
        System.out.println("Parent Contruct + " + name);
    }
}

public class Child extends Parent {
    
    public Child() {
        super("child"); // 부모 생성자 호출
    }
}
```
- 기본 생성자일 때도 보이지않지만 컴파일 시 super()를 통해 부모 생성자를 호출함
- 위 내용은 부모의 기본 생성자가 없기 때문에 명시적으로 호출 해줘야됨

<br>


### 2) 형 변환
```java
public class Parent {
    
    public void printName() {
        ...
    }
}

public class Child extends Parent {
    
    public void printName() {
        ...
    }
    
    public void printChildName() {
        ...
    }
}

public static void main(String[] args) {
    Parent parent = new Child(); // 가능
    Child obj = new Parent(); // 컴파일 에러 -> 자식은 부모로 선언이 안됨

    Child obj2 = (Child) parent; // 형 변환을 통해 명시적으로 선언 -> 실행 시 오류
    
    // ---------------------------------------
            
    Child child1 = new Child();
    Parent parent1 = child1; // parent 클래스지만 실제로는 Child 객체 
    Child child2 = (Child) parent1; // 문제 없이 실행
    
}
```
<br>

### 객체 구분을 위한 instanceof
```java
public static void main(String[] args) {
        Parent parent1 = new Parent();
        Child child1 = new Child();
         
        if(parent1 instanceof Parent){ // true
            System.out.println("parent");
        }

        if(parent1 instanceof Child) { // false
            System.out.println("parent");
        }
        
        if(child1 instaceof Parent) { // true
            System.out.println("child");
        }

        /**
         결과
         parent
         child
         */

        }
```
- 상속받은 타입도 true로 통과됨

<br>


### 3) 오버라이딩 ( Overriding )
```java
public class Parent {
    
    public void printName() {
        System.out.rpintln("ParentOverriding");
    }
}

public class Child extends Parent {
    
    public void printName() {
        System.out.println("ChildOverriding");
    }
}
```
- 부모 클래스의 정의된 메소드를 자식 클래스에서 동일한 메소드를 재정의
- 부모의 접근 제어자의 범위가 축소되면 안된다 ex) 부모 public, 자식 protected


### 다형성 ( Polymorphism )
- 자식 클래스는 자신만의 행위(메소드)를 가질 수 있고, 부모 클래스에 선언된 메소드는 공유가 가능

<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)

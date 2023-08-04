# Nested Class
- 클래스 안에 또다른 클래스
- 자바 기반의 UI 처리를 할 때 사용자의 입력, 외부의 이벤트에 대한 처리를 하는 곳에서 많이 사용

## 사용 목적
- 한 클래스에서 사용되는 클래스를 논리적으로 묶고 싶을 때
- 캡슐화를 통해 내부 구현을 감추고 싶을 때
- 소스의 가독성과 유지보수성을 높이고 싶을 때

> ex) 어떤 버튼을 눌렸을 때 수행해야하는 작업이 서로 다를 때 하나의 별도의 클래스를 만드는 것보단 내부 클래스로 만드는게 편함


## 종류
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/9ac3e672-3ee3-41f5-bd79-a9b2f0e31cd6" width="700" alt=""/>

### 1. Static nested
```java
public class OuterClass {
    private String name;
    
    static class StaticNested {
        ...
    }
}
```
```java
// 다른 클래스에서 해당 staticNested 생성
public void makeStaticNested() {
        OuterClass.StaticNested staticNested = new OutgerClass.StaticNested
}
```
- 외부의 클래스의 변수를 참조하는 것은 가능하지만 static 변수만 가능하다

<br>

### 2. Inner class
- 내부의 클래스는 외부 클래스의 어떤 변수도 접근 가능 (접근제어자 상관없이 가능하다)

#### 1) Local Inner Class
```java
public class InnerClass {
    class Inner {
        ...
    }
}
```
```java
// 다른 클래스에서 해당 innerNested 생성
public void makeLocalInner() {
    InnerClass innerClass = new InnerClass();
    InnerClass.Inner inner = innerClass.new Inner();
}
```

<br>

#### 2) Anonymous Class
```java
public class AnonymousSample {
    
    private EventListener listener;
    public void setListener(EventListener listener) {
        this.listener = listener;
    }
}

public void setEventListener() {
    AnonymousSample sample = new AnonymousSample();
    sample.setListener(new EventListener() {
        
        @Override
        public void onClick() {
            ...
        }
        
    });
}
```
- 이런 식으로 구현하게 되면 클래스의 이름, 객체 이름이 없기 때문에 익명 클래스라고 불린다
- 생성자 호출할 때 실제 구현 동작도 같이 적어준다
- 익명 클래스의 장점은 클래스를 많이 만들수록 메모리가 많이 필요해지는데 그 부분을 최소화할 수 있다
    ㄴ 

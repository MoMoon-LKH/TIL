# Generic
자바는 여러 타입들이 존재하기 때문에 형 변환을 하면 런타임 시 많은 예외가 발생할 수 있다. <br>
이런 경우를 막고자 컴파일 시 점검할 수 있도록 만든게 제네릭이다.

```java
public class CastingDto implements Serializable {
    private Object object;

    public void setObject(Object object) {
        this.object = object;
    }
    
    public Object getObject() {
        return object;
    }
}

public class GenericX() {
    
    public static void main(String[] args) {
        CastingDto dto1 = new CastingDto();
        dto1.setObject(new String());

        CastingDto dto2 = new CastingDto();
        dto2.setObject(new StringBuffer());
        
        String str = (String) dto1.getObject();
        StringBuffer strBf = (StringBuffer) dto2.getObject();
        
    }
}
``` 
위 코드를 통해 알 수 있는 것은 해당 객체를 꺼내올 때 각자에 맞는 형변환을 해줘야 정상적으로 사용이 가능하다는 점이다.

```java
public class CastingDto<T> implements Serializable {
    private Object object;

    public void setObject(T object) {
        this.object = object;
    }

    public Object getObject() {
        return object;
    }
}

public class GenericO() {

    public static void main(String[] args) {
        CastingDto dto1 = new CastingDto();
        dto1.setObject(new String());

        CastingDto dto2 = new CastingDto();
        dto2.setObject(new StringBuffer());

        String str = dto1.getObject();
        StringBuffer sb = dto2.getObject();
    }
}
```
위 코드는 getObject()라는 메소드를 통해 형변화 없이 가져올 수 있다. <br>
제네릭은 명시적으로 타입을 지정할 때 사용된다.

<br>

### 제너릭 타입으로 자주 쓰이는 문자
- E: 요소(Element) -> 자바 컬렉션에서 자주 사용
- K: 키
- N: 숫자
- T: 타입
- V: 값
- S,U,V: 두번째, 세번째, 네번째에 선언된 타입

<br>

## 제너릭의 ? 
```java

public class CastingDto<String> implements Serializable {
    private Object object;

    public void setObject(CastingDto<String> object) { 
        this.object = object;
    }

    public Object getObject() {
        return object;
    }
}

public class GenericQuestion {
    
    public static void main(String[] args) {
        CastingDto<String> dto = new CastingDto<String>();
        dto.setObject("String");

        GenericQuestion genericQuestion = new GenericQuestion();
        genericQuestion.callGenericQuestion(dto);
    }
    
    public void callGenericQuestion(CastingDto<?> c) {
        
        Object o = c.getObject(); // Object로 받을 수 밖에 없다. 
        // ?는 모든 타입을 허용하므로 부모 객체인 Object를 통해 받아야된다.
        
        if (o instanceof String) {
            System.out.println(o);
        }
    }
}
```
제너릭 타입의 ?는 모든 타입을 받을 수 있다. <br>
하지만 매개변수로 받을 때는 어떤 타입으로 올지 모르기 때문에 Object로 처리해야된다.
- 매개변수로만 사용하는 것을 권장한다
<br>

## 제네릭 타입 범위 선언
```java
public class Car {
    protected String name;
    
    public Car(String name) {
        this.name = name;
    }
    
    public String toString() {
        return "name = " + name;
    }
}

public class Bus extends Car {
    public Bus(String name) {
        super(name);
    }

    public String toString() {
        return "Bus name = " + name;
    }
}

public class CarWildSample {
    
    public static void main(String[] args) {
        CarWildSample sample = new CarWildSample();
        CastingDto<Car> dto = new CastingDto();
        dto.setObject(new Bus("A"));
        sample.boundWildMethod(dto);
        
    }
    
    public void boundWildMethod(CastingDto<? extends Car> c) {
        ...
    }
}

```
위 boundedWildMethod의 매개변수를 보면 CastingDto<? extends Car> c 라고 되어있다. <br>
해당 구문은 제네릭 타입으로 Car를 상속받은 객체만 받겠다는 것이다.


## 제네릭 메소드
```java
public class GenericClass<T> {
    private T generic;

    public GenericClass(T t) {
        generic = t;
    }

    public void setGeneric(T t) {
        generic = t;
    }

    public T getGeneric() {
        return generic;
    }
}

public class GenericMethodSample {

    // generic Method
    public <T> void genericMethod(GenericClass<T> c, T addValue) {
        c.setGeneric(addValue);
        T value = c.getGeneric();
        System.out.println(value);
    }
}

public static void main(String[] args) {
    GenericMethodSample methodSample = new GenericMethodSample();
    GenericClass<String> genericClass = new GenericClass<String>("A");
    methodSample.genericMethod(genericClass, "data");
}

//ex 제너릭 타입이 두 개인 경우
public<S, T extends Car> void multiGeneric(GenericClass<T> t, T addValue, S another)

```
리턴타입 앞에 제네릭을 선언함으로써 제네릭 메소드로 사용할 수 있다


<br>


### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)


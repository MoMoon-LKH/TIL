## 객체
### 객체란?
    각각의 실제 사물을 나타내기 위한 것
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
<br>

### 클래스와 객체의 다른점
    클래스의 생성자를 통해 객체를 생성
    즉, 클래스는 설계도고 객체는 설계도를 통해 생성된 각각의 사물
    
<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)

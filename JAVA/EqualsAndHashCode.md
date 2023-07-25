# Equals()와 hashCode()

## 1. Equals()
### 참조 자료형 (객체) 비교 메소드
```java
public boolean equals(Object obj) {
    return (this == obj);
}
```
- 오버라이딩을 하지않으면 hashCode()를 통해 주소 값을 비교한다 (동일성)
  - 각각의 동일한 값의 생성자로 생성했을 때 서로의 해시 코드가 다르므로 다르다 
  - 상속 클래스에서 오버라이드하여 사용하기 때문에 동등성 비교로 보임

#### 객체의 동등성을 비교하기위해선 hashCode 또한 주소값을 기준으로 비교하는게 아닌 값을 기준으로 비교해야된다

### Equals()를 사용하기 위해선 hashCode()도 같이 오버라이딩 해줘야한다

<br>

------------------------------


## 2. hashCode()
### 해당 객체의 메모리 주소를 고유의 integer 값으로 변환 후 반환 
- 서로 동일한 객체가 있다면 hasCode()의 값은 무조건 동일

### 규칙
 1. 동일한 객체는 항상 동일한 int 값을 리턴해주어야된다
 2. equals() 비교값이 true인 경우 hashCode()도 동일한 해시값을 리턴해야된다

<br>


------------------------------

## equals()와 hashCode()의 관계
    동일한 객체는 동일한 주소를 가지므로 동일한 해시코드를 가져야한다
    -> 오버라이드를 했다면 동일한 값을 가진 객체는 동일한 해시코드를 가져야한다

### 해시 컬렉션
#### equals만 Override 했다고 가정 
```java
public void example() {
    
    Car car1 = new Car("car");
    Car car2 = new Car("car");
    
    if(car1.equals(car2)) { // Override를 통해 동등성으로 비교했으므로 true
        System.out.println("true");     
    }
    
    Set<Car> cars = new HashSet<Car>();
    cars.add(car1);
    cars.add(car2);
    
    System.out.println(cars.size() == 1) // false

    /**
     * 의도대로라면 size가 1이 나와서 true true가 나와야 정상적인 동작이다
     * 하지만 car1, car2는 생성자를 통해 각자 생성했으므로 다른 주소를 가진다
     * Override 하지않은 hashCode()의 경우 주소값을 해시하기때문에 HashSet에선 동일한 객체라고 판단하지않는다.
     */    
    
}
```
### 따라서 equals()를 오버라이드했다면 hashCode()도 마찬가지로 값을 기준으로 고유 값을 반환해야된다

<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)



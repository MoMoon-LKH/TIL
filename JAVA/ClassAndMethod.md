# 클래스와 메소드
## 메소드
    클래스의 행위에 해당되며 타입과 이름을 가진 매개변수를 받고 리턴타입에 해당되는 결과값을 반환해주는 함수 
### 1) 구조
```java
public boolean checkPassword(String password) {
접근제어자 ㄴ리턴타입 메소드 이름    매개변수
    // 메소드 내용
}

```
### 메소드는 반드시 클래스 안에서 정의되어야 한다


## 클래스
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


### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)
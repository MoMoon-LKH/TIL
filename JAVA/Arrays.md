# 배열
```java
int[] example = new int[5];
```
### 특징
- 참조 자료형 (주소)
- 참조형이기 때문에 new 예약어를 통해 생성
    - { }로 선언이 가능하지만 변수 선언 및 초기화가 동시에 이루어져야됨<br> 
      (보통 변경되지않은 값을 초기화하는데 사용)
- 시작 위치는 0부터 시작

### 기본값
#### 기본 자료형 
- 각 자료형의 기본값

#### 참조 자료형
- null
<br>


### 2차원 배열
```java
int[][] exaples = new int[3][];
```

<br>

### 배열 사용 시 좋은 코드
```java

public void print() {
    int[] array = {1, 2, 3, ,4};
    
    for(int i = 0; i < array.length; i++){ // for 루프가 수행될 때마다 length로 길이를 가져옴
        ...    
    }    
}

// 리펙토링 코드
public void print() {
    int[] array = {1, 2, 3, ,4};
    int len = array.len // 미리 가져옴
    
    for(int i = 0; i < len; i++){ 
        ...
    }
}


public void print() {
    int[] array = {1, 2, 3, ,4};

    for(int num : array){ //순서대로 돌면서 루프 할당
        ...
    }
}


배열의 값을 for 루프의 조건에 직접적으로 입력하는 코딩은 피하는 것이 좋다
```
<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)


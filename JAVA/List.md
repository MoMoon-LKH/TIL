# List
## List와 Vector
- Collection 인터페이스에서 확장되었다
- 순서가 있다

ArrayList와 Vector 클래스의 사용법은 동일하고 기능도 거의 비슷하다. <br>
하지만 ArrayList는 Thread unsafe하고 Vector는 Thread safe하다.
<br>

--------------------------------------------------------------------

## ArrayList
- 확장이 가능한 배열


### ArrayList 생성자
#### 1) ArrayList()
- 객체를 저장할 공간이 10개인 ArrayList 생성

#### 2) ArrayList(Collection<? extends E> c)
- 매개 변수로 넘어온 컬렉션 객체가 저장되어 있는 ArrayList 생성

#### 3) ArrayList(int initailCapacity)
- 매개 변수로 넘어온 값만큼 저장 공간을 갖는 ArrayList 생성
<br>

### 특징
- 배열처럼 사용하지만 메소드를 통해 동작한다
- 한 종류의 객체만 저장
- default size = 10
- 크기가 넘어가는 데이터가 들어가면 자동으로 크기를 늘리는 작업을 수행 <br>
  ㄴ 어느정도 예측이 가능한 크기가 있다면 예측한 크기만큼 지정할 것을 권장
<br>

### 메소드
#### 1) add(E e)
| 리턴타입| 메소드 이름                                       | 설명                       |
|:------:|:---------------------------------------------|:-------------------------|
|boolean| add(E e)                                     | 해당 데이터를 가장 끝에 담는다        |
|void| add(int index, E e                           | 해당 데이터를 지정한 index 위치에 담는다|
|boolean| addAll(Collection<? extends E> c)            |해당 컬렉션 데이터를 가장 끝에 담는다|
|boolean| addAll(int index, Collection<? extends E> c) |지정한 index부터 해당 컬렉션 데이터를 담는다| 

<br>


#### 2) get
| 리턴타입| 메소드 이름                                       | 설명                       |
|:------:|:---------------------------------------------|:-------------------------|
|E|get(int index)|해당 index 위치에 있는 데이터를 가지고온다|
|int|indexOf(Object o)|해당 객체와 동일한 데이터의 위치를 가져온다|
|int|lastIndexOf(Object o)|해당 객체와 동일한 마지막 데이터 위치를 가져온다|

<br>

#### 3)delete
| 리턴타입| 메소드 이름                     | 설명                       |
|:------:|:---------------------------|:-------------------------|
|void| clear()                    |모든 데이터 삭제|
|E| remove(int index)          |지정된 index의 데이터를 삭제|
|boolean| remove(Object o)           |해당 객체와 동일한 첫 번째 데이터 삭제|
|boolean| removeAll(Collection<?> c) |해당 컬렉션 객체에 있는 데이터와 동일한 모든 데이터 삭제|

<br>

#### 4) size()와 length 차이
size()는 해당 ArrayList 객체가 가지고 있는 데이터의 개수이다. <br>
반면에 length는 해당 ArrayList 객체의 할당한 크기를 말한다.

<br>

----------------------------------------------------------------------------

## Stack
- Vector 클래스에서 확장
- LIFO( Last In First Out)
- Thread safe

### 1) push
1 -> 2 -> 3 로 순서대로 데이터를 넣으면 <br><br>
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/4b4f0aea-fd44-47a7-9bb0-4e68d56f6b8a" width="500">

위 그림처럼 데이터가 들어가게된다
```java
public class Main {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(1);
        stack.push(2);
        stack.push(3);
        System.out.println(stack);
    }
   /*
        결과
        [1, 2, 3]
    */
}
```

### 2) pop
Stack는 LIFO 성질로 인해 가장 나중에 들어간 값을 먼저 가져오고 제거한다

<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/4cb85a3f-4333-4ef9-9c24-df12a2772ec0" width="300" />

```java
public class Main {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(1);
        stack.push(2);
        stack.push(3);
        System.out.println(stack);

        stack.pop();
        System.out.println(stack);
    }

   /*
        결과
        [1, 2, 3]
        [1, 2]
    */
}
```

### 3) peek
peek() 메소드는 pop()과 달리 가장 위의 있는 값을 가져오기만 한다

<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/f867d082-328d-4cda-81a2-0273f30cfc15" width="150">

```java
public class Main {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(1);
        stack.push(2);
        stack.push(3);
        System.out.println(stack);
        System.out.println(stack.peek());
   }

   /*
        결과
        [1, 2, 3]
        3
    */
}
```

<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)


# Set와 Queue

# Set
- 어떤 데이터가 존재하는지를 확인하기 위한 용도

## 특징
- 중복을 포함하지 않는다 -> 여러 중복되는 값을 걸러낼 수 있다
- 순서에 상관없다

## 종류
### 1) HashSet
- 순서가 전혀 없는 데이터를 해시 테이블에 저장 
- 성능이 가장 좋다

```java
public static void main(String[] args) {
    Set<String> set = new HashSet<>();
    System.out.println(set.isEmpty()); // 데어터 있는지 확인
    set.add("A"); // 데이터 추가
    System.out.println(set.contains("A")); // 데이터 존재 확인
    set.remove("A"); // 데이터 삭제
}
```


### 2) TreeSet
- 저장된 데이터의 값에 따라서 정렬되는 Set
- red-black이라는 트리 타입(이진 트리)으로 저장
- HashSet 보다 성능이 느리다

### 3) LinkedHashSet
- 연결된 목록 타입으로 구현된 해시 테이블에 데이터를 저장
- 저장된 순서에 따라서 값이 정렬된다
- 성능이 가장 안좋다


## 주의점들

#### 로드 팩터(load factor)
(데이터 개수)/(저장 개수) 
- 만약 데이터의 개수가 증가하여 로드 팩터보다 커지면, 저장 공간의 크기는 증가되고 해시 재정리 작업 실행 <br>
  ㄴ 해당 기준(default 0.75)을 넘으면 내부에 갖고 있는 자료 구조를 다시 생성하는 단계를 거쳐야 되므로 성능에 영향이 있다
<br>

#### equals()와 hashCode()
Set에서 데이터가 중복되는 걸 허용하지 않는다. 따라서 데이터를 확인하는 부분이 중요한데 <br>
이부분을 equals()와 hashCode()에서 하기 때문에 해당 구현이 중요하다


<br>

# Queue
- FIFO (First In First Out) 선입선출 자료 구조
- Linked List의 상위 클래스

<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/508f9e34-2edd-41b5-8ad5-7b61f70e6fad" width="700">


## Linked List
- 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 형태
- List 될 수 도 있고 Queue가 될 수도 있다
- 배열 중간에 있는 데이터가 지속적으로 삭제되거나 추가될 경우 메모리 공간 측면에서 유용하다 <br>
  ㄴ 결국 앞 뒷에 있는 데이터만 연결시키면 되기 때문에 효과적이다

<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/d48ebe41-abcc-4796-8649-278e179a9967" width="700">

### 메소드 같은 기능 중복
LinkedList의 메소드를 보면 중복된 기능을 가진 메소드가 많다 <br>
이 경우는 LinkedList가 여러 종류의 인터페이스를 구현했기 때문이다. <br>
아래 메소드는 다른 메소드에서도 해당 메소드를 불러 구현했기 때문에 해당 메소드를 사용하기를 권장한다
- 데이터 저장 = 맨 앞 추가일 경우 addFirst(Object), 맨 뒤일 경우 add(Object) or addLast(Object)
- 데이터 리턴 = getFirst()
- 데이터 삭제 = removeFirst() 또는 removeLast()


<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)


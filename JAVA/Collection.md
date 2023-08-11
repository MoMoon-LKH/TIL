# Collection
## 자료구조
어떤 정보를 담은 것을 의미하며 하나의 데이터가 아닌 여러 데이터를 담은 것을 말한다

### 필요 이유
기본적으로 배열이 있지만 배열은 그 크기가 정해져 있을 때 유용한 경우이다. <br>
데이터의 크기가 얼마나 될지 모르는 상황이 있을 수 있는데 배열로 처리하려면
- 배열의 크기를 엄청나게 늘린다 -> 메모리 낭비로 이어진다.
- 배열의 크기가 부족하면, 필요한 개수만큼 더 큰 배열을 하나 더 만들어서 복사한다

위 경우를 비롯한 여러 답은 있겠지만 위 같은 상황에서 유용하게 사용할 수 있는 자료구조가 존재한다.
<br>

## 종류
- 순서가 있는 목록 List
- 순서가 중요하지 않은 Set
- 먼저 들어온 것이 나가는 Queue
- 키-값으로 이루어진 Map

<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/c9e220c6-f687-43ed-9538-e533f88bdc1c" width="800">
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/f87a8a90-1749-473a-99ad-7a5b2cc5113b" width="800">

<br>

### Collection
```java
public interface Collection<E> extends Iterable<E>
```
- Iterable 인터페이스를 확장 -> Iterable 인터페이스를 통해 데이터를 순차적으로 가져올 수 있다.



### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)


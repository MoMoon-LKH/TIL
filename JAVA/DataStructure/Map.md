# Map

## 특징
- 키(Key)와 값(Value)이 1:1로 이루어진 데이터 구조
- 키는 해당 Map에서 고유값


## 종류
### 1) HashMap
```java
public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("A", "a"); // 데이터 추가
        String a = map.get("A"); // 데이터 확인
        map.remove("A"); // 데이터 삭제
    }
}
```
- 담을 데이터의 개수가 많은 경우 초기 크기를 지정해주는 것을 권장
- 키는 기본 자료형, 참조 자료형 둘 다 된다
- 존재하지 않은 키로  get()할 경우 null 리턴
- 이미 존재하는 key라면 기존의 값을 새로운 값으로 대체

#### Entry Object
```java
public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("A", "a");
        map.put("B", "b");
        map.put("C", "c");
        map.put("D", "d");

        Set<Map.Entry<String, String>> entries = map.entrySet();
        for(Map.Entry<String, String> tmp : entries) {
            System.out.println(tmp.getKey() + " = " + tmp.getValue());
        }

    }
}
```
<br>

#### 좋은 코드
```java
public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("A", "a");
        map.put("B", "b");

        if (map.containsKey("A")) { // get으로 가져오기 전에 containKey()나 containValue()를 통해 확인
            System.out.println(map.get("A"));
        }
    }
}
```

### 2) TreeMap
- 키를 정렬하여 저장한다
- 정렬 순서 : 숫자 > 알파벳 대문자 > 알파벳 소문자 > 한글

```java
public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new TreeMap<>();
        map.put("A", "a");
        map.put("1", "0");
        map.put("문", "ㅇ");
        map.put("c", "d");

        for (Map.Entry<String, String> tmp : map.entrySet()) {
            System.out.println(tmp.getKey() + "=" + tmp.getValue());
        }
    }
    
    /*
        1=0
        A=a
        c=d
        문=ㅇ
     */
}
```

### 3) LinkedHashMap
<br>

## Map과 Hashtable 차이
|기능|HashMap|Hashtalbe|
|:------|:--------------:|:-------:|
|키나 값에 null 저장여부|가능|   불가능   |
|멀티 쓰레드 안전 여부| 불가능| 가능|


<br>

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)


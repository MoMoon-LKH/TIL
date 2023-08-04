# Annotation
## 사용 이유
- 컴파일러에게 정보 제공
- 컴파일와 설치 시 작업을 지정
- 실행할 때 별도의 처리가 필요할 때


## 종류
### 1. @Override
- 부모 클래스의 메소드를 Override했다는 것을 명시적으로 선언
- Override 했다는 메소드라고 표시

```java
@Override
public void printName(){ 
        ...
}
```

### 2. @Deprecated
- 클래스나 메소드가 더이상 사용되지 않는 경우 사용
- 컴파일 시 경고 메시지 출력

```java
@Deprecated
public void printName() {
        ...    
}
```

### 3. @SuppressWarnings
- 경고 메시지를 무시하고 싶을 때 사용
```java
@SuppressWarnings("deprecation")
public void printName() {
        ...
}
```
<br>

### 4. 메타 어노테이션
- 개발자가 따로 선언하여 사용

#### 1) @Target
- 어떤 것에 적용할지 선언
```java
@Target(ElementType.METHOD)
```
|    요소타입     |             대상              |
|:-----------:|:---------------------------:|
| CONSTRUCTOR |           생성자 선언시           |
|   FFIELD    |enum 상수를 포함한 필드(filed) 값 선언시 |
|LOCAL_VARIABLE|          지역변수 선언시           |
|METHOD| 메소드 선언시|
|PACKAGE|패키지 선언시|
|PARAMETER|매개 변수 선언시|
|TYPE| 클래스, 인터페이스, enum 선언시|

#### 2) @Retention
- 어노테이션 정보가 유지되는 사이클

| |대상|
|:-------:|:---------:|
|SOURCE|컴파일시 사라짐|
|CLASS|클래스파일에 있는 어노에이션이 컴파일러에 의해서 참조, JVM에서는 사라짐|
|RUNTIME|실행시 어노테이션 저옵가 가상머신에 으ㅐ해서 참조 가능|

#### 3) @Documented
- 해당 어노테이션 정보가 JavaDocs 문서에 포함된다는 것

#### 4) @Inherited
- 모든 자식 클래스는 부모 클래스의 어노테이션을 사용 가능하다는 것을 선언

### 상속
- 어노테이션은 상속이 불가능하다 

### 참고
[자바의 신 - 이상민 저](https://www.yes24.com/Product/Goods/42643850)
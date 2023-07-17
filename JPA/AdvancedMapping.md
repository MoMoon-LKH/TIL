# 고급매핑
## 1. 상속관계 매핑
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/6f4dbe46-6860-412c-a587-777b03d6fd88" width="600">

### 1) 조인
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/d3690b34-a05e-4179-97c9-36a4d9a63c9a" width="600">
    
    - 자식 테이블이 부모의 기본키를 기본키 + 외래키로 사용하는 전략
    - 조회 시 조인을 많이 씀
    - 구분 컬럼을 사용

```java
@Entity
@Ingeritance(strategy = inheritanceType.JOINED) // 상속 매핑 -> 조인 전략을 사용하겠다 선언
@DiscriminatorColumn(name = "DTYPE") // 구분컬럼 지정 
public abstract class Item {
    ...
}

@Entity
@DiscriminatorValue("A") // 구분 컬럼에 입력할 값을 지정
public class Album extends Item {
    ...
}

```

#### 장점
    - 테이블이 정규화됨
    - 외래 키 참조 무결성 제약조건을 활용 가능
    - 저장공간을 효율적으로 사용
#### 단점
    - 조회 시 조인이 많이 사용되어 성능 저하 우려
    - 조회 쿼리 복잡도 상승
    - 데이터 등록 시 INSERT SQL 두번 실행
<br>

### 2) 단일 테이블
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/1aae70db-9f68-407c-ace9-e07f407682e0" width="600">
    
    - 테이블을 하나만 사용하는 전략
    - 자식 엔티티가 매핑한 컬럼은 모두 null 값을 허용해야됨
    - 구분 컬럼을 꼭 사용해야 한다

```java
@Entity
@Ingeritance(strategy = inheritanceType.SINGLE_TABLE) // 상속 매핑 -> 단일 테이블 전략
@DiscriminatorColumn(name = "DTYPE") // 구분컬럼 지정 
public abstract class Item {
    ...
}

@Entity
@DiscriminatorValue("A") // 구분 컬럼에 입력할 값을 지정
public class Album extends Item {
    ...
}

```

#### 장점
    - 조회 성능이 좋다
    - 조회쿼리 간략화

#### 단점
    - 자식이 매핑한 컬럼 모두 null 허용해야됨
    - 하나의 테이블에 다 저장하므로 테이블이 커질 수 있음 -> 조회 성능 저하 우려

<br>

### 3)구현 클래스마다 테이블
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/cdd4b53b-945e-44ce-ab9c-adf3066ccf52" width="600">

    - 구현 클래스마다 테이블을 생성하는 전략
    - 일반적으로 추천하지 않는 전략
    - 구분 컬럼을 사용하지 않음
```java
@Entity
@Ingeritance(strategy = inheritanceType.TABLE_PER_CLASS) // 상속 매핑 -> 구현 클래스 테이블 전략 선언
public abstract class Item {
    ...
}

@Entity
public class Album extends Item {
    ...
}

```
#### 장점
    - 서브 타입을 구분해서 처리할 때 효과적
    - not null 제약조건 사용 가능

#### 단점
    - 여러 자식 테이블을 함께 조회 할때 성능이 안좋다
    - 자식 테이블을 통합해서 쿼리하기 어렵다

<br>

## 2. @MappedSuperclass (공통 속성 정의 클래스)
- 부모 클래스의 매핑 정보를 상속받는 자식 클래스에게 제공하고 싶으면 사용

<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/4724f508-81a8-4490-9167-cd27b1fd4147" width="600">

```java
@MappedSuperclass
public abstract class BaseEntity {
    @Id @GeneratedValue
    private Long id;
    
    private String name;
}

@Entity
public class Member extends BaseEntity {
    // ID 상속
    // NAME 상속
    private String email;
}

@Entity
public class Seller extends BaseEntity {
    // ID 상속
    // NAME 상속
    private Strign shopName;
}

```
#### 상속받은 매핑 정보를 재정의 하고 싶다면
```java
@AttributeOverrides({
        @AttributeOverride(name = "id", column = @Column(name = "MEMBER_ID")),
        @AttributeOverride(name = "name", column = @Column(name = "MEMBER_NAME"))
})
public class Member extends BaseEntity {}
```
<br>

## 3. 복합 키와 식별 관계 매핑
### 1) 식별관계
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/6101e05d-06b4-43d3-bd99-d4a9f7b96ea7" width="600">

    부모 테이블 기본키 -> 상속 -> 자식 테이블 기본키 + 외래키로 사용 

```java
@Entity
public class Parent {
    @Id @Column(name = "PARENT_ID")
    private String id;
    ...
}

public class ChildId implements Serializable {
    private String parent;
    private String childId;
}

// @IdClass 경우
@Entity
@IdClass(ChildId.class)
public class Child {
    
    @Id
    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    public Parent parent;
}


// EmbeddedId 경우
@Embeddable
public class ChildId implements Serializable {
    
    private String parentId;
    
    @Column(name = "CHILD_ID")
    private String childId;
}

@Entity
public class child {
    @EmbeddedId
    private String childId;
    
    @MpasId("parentId")
    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    public Parent parent;
    ...
}

```
<br>

### (1) 일대일 식별관계 경우
- 자식 테이블의 기본키 -> 부모의 기본값을 사용
```java
@Entity
public class Board {
    @Id @GeneratedValue
    @Column(name = "BOARD_ID")
    private Long id;
    
    @OneToOne(mappedBy = "board")
    private BoardDetail boardDetail;
}

@Entity
public class BoardDetail {
    
    @Id
    private Long boardId;
    
    @MapsId
    @OneToOne
    @JoinColumn(name = "BOARD_ID")
    private Board board;
    ...
}

```

<br>

### 2) 비식별관계
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/4767cbdf-c5b0-4377-8845-ad4b99023da8" width="600">

    부모 테이블 기본키 -> 상속 -> 자식 테이블 외래키로만 사용

```java
@Entity
public class Hello {
    @Id 
    private String id1;
    
    @Id
    private String id2; // 예외 발생
}

/*
        @Id가 두 개인 경우 식별자 구분이 안되는 상태이기 때문에 에러 발생
        밑에 주의점 참고할 것 
*/
```
<br>

### (1) IdClass (복합키)
```java

@Entity
@IdClass(ParentId.class)
public class Parent {
    
    @Id @Column(name = PARENT_ID1)
    private String id1;
    
    @Id @Column(name = PARENT_ID2)
    private String id2;
}

public class ParentId implements Serializable {
    private String id1;
    private String id2;

    @Override
    public boolean equals(Object object) {..}
    
    @Override
    public int hashCode() {..}
}
```
- 식별자 클래스의 이름과 엔티티 이름이 같아야됨

<br>

### (2) EmbeddedId (복합키)
```java

// parentId 동일

@Entity
public class Parent {
    
    @EmbeddedId
    private ParentId id;
}

```
- 식별자 클래스를 기본 키에 직접 매핑



### [ ! ] 주의점 equals(), hashCode()
    복합키에서는 equals()와 hashCode()를 필수로 Override 해야한다
    - equals()는 Object 클래스 비교에서는 동일성을 비교한다 (주소 비교)
    
    하지만 영속성 컨텍스트는 엔티티의 식별자를 키로 사용해서 엔티티를 관리 (값 비교)
    즉 동일성이 아닌 동등성으로 비교해야되는데 equals(), hashCode()의 경우는 동일성으로 비교한다
    따라서 equals(), hashCode()를 @Override를 통해 동등성으로 구현해야됨
<br>

### (3) 비식별관계 구현
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/e8b0f2e6-0270-4f87-80e0-7ef3ab1cbb22" width="600">

```java
@Entity
public class Parent {
    @Id @Column(name = "PARENT_ID")
    private String id;
    ...
}

@Entity
public class Child {
    
    @Id @GeneratedValue
    @Coulmn(name = "CHILD_ID")
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    private Parent parent;
    
    ...
}
```


<br>

## 참고
[자바 ORM 표준 JPA 프로그래밍 - 김영한 저자](https://product.kyobobook.co.kr/detail/S000000935744)

2023-07-16
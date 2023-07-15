## 연관관계 다중성

## 1) 다대일
    객체의 양방향 관계에서 연관관계의 주인은 항상 다쪽
    
### (1) 단방향 [N:1]
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/66117570-2952-4fbf-a16e-19c658189225" width="500">
<br>  
- 회원 -> 팀 참조 가능, 팀 -> 회원 참조할 필드가 없음<br>


### (2) 양방향 [N:1, 1:N]
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/e709f17d-332f-4d81-a494-b77fdd84ffaf" width="500">
<br>

```java
public class Member {
    ...

    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
    
    ...
}

public class Team {
    ...
    
    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<Member>();
    
    ...
}

```
- 양방향에서의 연관관계는 항상 다(N)
- 항상 서로 참조해야된다

## 2) 일대다
- 엔티티를 하나 이상 참조 가능하므로 Collection, List, Set, Map 중 하나 사용

### (1) 단방향 [1:N]
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/80794480-5a0b-460e-9d45-37c5d25d143a" width="500">

```java
public class Team {
    ...
    
    @OneToMany
    @JoinColumn(name = "TEAM_ID")
    private List<Member> members = new ArrayList<Member>();
            
    ...
}
```
- @JoinColumn을 명시해야됨 <br>
  ㄴ 명시안하면 연결 테이블을 중간에 두고 연관관계를 관리하는 조인 테이블 전략을 기본으로 사용하여 매핑
- 단점으로 매핑한 객체가 관리하는 외래 키가 다른 테이블에 있음 <br>
  ㄴ 다른테이블에 외래키가 있는 경우 연관관계 처리를 위해 UPDATE SQL를 추가로 실행해야됨

#### ex) 추가 Update 실행문
```java
public void test() {
    
    Member member1 = new Member("member1");
    
    Team team1 = new Team("team");
    team1.getMembers().add(member1);
    
    em.persist(member1);
    em.persist(team1); // insert team1 , update member1.fk
    
    transaction.commit();
}

// Member의 team_id를 update 시켜줘야됨
```

 
### (2) 양방향 [1:N, N:1]
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/805485dc-3781-47e5-ae3b-c091fde73a43" width="500">

```java
public class Team {
    ...

    @OneToMany
    @JoinColumn(name = "TEAM_ID")
    private List<Member> members = new ArrayList<Member>();
    
    ...
}

public class Member {
    ...
    
    @ManyToOne
    @JoinColumn(name = "TEAM_ID", insertable = false, updatable = false)
    private Team team;
            
    ...
}

```
- 외래키를 둘다 관리하므로 문제 발생할 가능성 큼
- 다대일쪽은 insertable = false, updatable = false로 설정하여 읽기만 가능
#### 일대다 양방향은 일대다 단방향 매핑이 가지는 단점을 그대로 가짐 -> 다대일 매핑 사용 권장

<br>

## 3) 일대일
- 서로 하나의 관계만 가짐

### (1) 주테이블에 외래키
- 객체지향 개발자분들이 선호
#### 단방향
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/ca25c14e-6ba8-40f9-b24f-3a623c981507" width="500">

```java
public class Member {
    ...
    
    @OneToOne
    @JoinColumn(name = "LOCKER_ID")
    private Locker locker;
            
    ...
}
```
<br>

#### 양방향
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/363eaeeb-c3f9-4772-abbb-974d08ad251a" width="500">

```java
public class Member {
    ...
    
    @OneToOne
    @JoinColumn(name = "LOCKER_ID")
    private Locker locker;
    
    ...
}

public class Locker {
    ...
    
    @OneToOne(mappedBy = "locker")
    private Member member;
    
    ...
}
```

### (2) 대상 테이블에 외래키
- 데이터베이스 개발자분들이 선호

#### 단방향
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/dfca85a4-5d7c-40f3-8784-b70525117fa8" width="500">

- JPA에서는 지원해주지 않음

<br>

#### 양방향
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/5f05a307-c14d-4459-9980-4d9a08142664" width="500">

```java
public class Member {
    ...
    @OneToOne(mappedBy = "member")
    private Locker locker;
    ...
}

public class Locker {
    ...
    @OneToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;
    ...
}
```


<br>

## 4) 다대다
- 보통 연결테이블로서 일대다, 다대일로 풀어내서 사용

<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/5e196a6d-d360-4c5b-9ab9-e1eaf07173ca" width="500">

#### 단방향
```java
public class Member {
    ...
    @ManyToMany
    @JoinTable(name = "MEMBER_PRODUCT",
        joinColumns = @JoinColumn(name = "MEMBER_ID"),
        inverseJoinColumn = @JoinColumn(name ="PRODUCT_ID"))
    private List<Product> products = new ArrayList<>(); // 보통 사용하지않은 방법, 확장성이 없음
    
}

```
<br>

#### 양방향
```java
public class Product {
    ...
    @ManyToMany(mappedBy = "products") // 역방향 추가
    private List<Member> members;
}
//위 코드에서 추가
```

### 연결 엔티티 사용
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/8431482d-7beb-4d61-8f51-eb74e4c810d7" width="500"> <br>
- 확장성이 있음
<br><br>

#### 1) 복합 기본키 방법 (식별 관계)
- 두 기본키를 묶어서 복합 기본키로 사용
```java
public class Member {
    ...
    @OneToMany(mappedBy = "member")
    private List<MemberProduct> memberProducts;
    ...
}

public class Product {
    ...
}

public class MemberProductId implements Serializable{
    
    private String member;
    private String product;
    
    @Override
    public boolean equals(Object o) {...}
    
    @Override
    public int hashCode() {...}
    
}


@IdClass(MemberProductId.class) //@EmbeddedId로 대체 가능
public class MemberProduct {
    
    @Id
    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;
    
    @Id
    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product;
    
    private int orderAmount;
    
    ...
}
```
- 별도의 public 식별자 클래스를 작성해야됨
- Serializable을 구현해야됨
- equals와 hashCode 메소드르 구현해야됨
- 기본생성자가 있어야됨
<br>

#### 2) 대리키 방법 (비식별 관계)
```java
@Entity
public class Order {
    
    @Id @GeneratedValue
    @Column(name = "ORDER_ID")
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;
    
    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product;
}

```
- 간편하며 영구적임

### 비식별관계를 권장


## 참조
[자바 ORM 표준 JPA 프로그래밍 - 김영한 저자](https://product.kyobobook.co.kr/detail/S000000935744)

2023-07-15
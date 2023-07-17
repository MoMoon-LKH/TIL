# 연관관계 관리
## 1. 즉시로딩과 지연로딩
### 기본 패치 전략
    연관된 엔티티가 하나 -> 즉시 로딩
    컬렉션 -> 지연 로딩

### 1) 즉시 로딩
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/f71a15fa-0035-422b-8db0-b7b602c13b55" width="500">

    - 엔티티를 조회할 때 연관된 엔티티도 함께 조회
    - 조인 쿼리를 사용해서 조회

```java
@Entity
public class Member {
    ...
    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "TEAM_ID")
    private Team team;
    ...
}
```

#### (1) join 방식
    기본적으로는 LEFT OUTER JOIN을 한다
    하지만 @JoinColumn(nullable = false)를 달아주게되면 INNER JOIN을 한다  
<br>

### 2) 지연로딩
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/2f55d6d5-cd09-40da-9e22-bbc287e2c707" width="500">

    - 연관된 엔티티를 실제 사용할 때 조회

```java
@Entity
public class Member {
    ...
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "TEAM_ID")
    private Team team;
    ...
}

public void test() {
    em.find(Member.class, "member1");
    team.getName() // 호출로 프록시 객체가 초기화 (영속성 컨텍스트에 없을 경우)
}

// 영속성 컨텍스트에 이미 로딩되어 있다면 실제 team 객체를 사용
```

### 3) 컬렉션 래퍼
    엔티티을 영속 상태로 만들 때 컬렉션이 있다면 컬렉션을 추적하고 관리할 목적으로 만든 내장 컬렉션
    ㄴ 하이버네이트에서 제공하는 내장 컬렉션으로 변경

```java
// 프록시 초기화 조건
member.getOrders(); // 초기화 되지않음
member.getOrders().get(0) // 실제데이터를 조회할 때 초기화
```
<br>

### 그러면 전반적으로 어떤 fetch 전략을 추천하는가? = 지연로딩
    특정 회원이 연관된 컬렉션에 저장된 데이터가 수만건일 수 있다
    그러면 해당 회원을 조회 시 데이터가 같이 로딩 -> 성능 부하 
    ㄴ 꼭 필요하다고 생각되는 곳에만 즉시 로딩
<br>

## 2. 영속성 전이 (CASECADE)
    특정 엔티티를 영속 상태로 만들 때 연관된 엔티티를 영속 상태로 만드는 기능

### 저장 
```java
@Entity
public class Parent {
    ...
    @OneToMany(mappedBy = "parent", casecade = CasecadeType.PERSIST)
    private List<Child> children = new ArrayList<>();
    ...
}

public void saveWithCasecade(EntityManager em) {
    
    Parent parent = new Parent();
    Child child = new Child();
    
    child.setParent(parent);  // 연관관계 추가
    
    parent.getChildren().add(child);
    
    // 부모 저장, 연관된 자식 저장
    em.persist(parent);
}

```

### 삭제
```java
@Entity
public class Parent {
    ...
    @OneToMany(mappedBy = "parent", casecade = CasecadeType.REMOVE)
    private List<Child> children = new ArrayList<>();
    ...
}

public void saveWithCasecade(EntityManager em) {
    
    Parent parent = em.find(Parent.class, 1L);
    Child child = em.find(child.class, 1L);
    
    em.remove(parent);
    // 부모 엔티티만 삭제하면 연관된 자식 엔티티도 함께 삭제
}

```
<br>

## 고아 객체
    부모 엔티티와 연관관계가 끊어진 자식 엔티티 객체
    ㄴ 끊어진 자식 엔티티 객체를 자동으로 삭제하는 기능을 제공
- 참조하는 곳이 하나일 때만 사용

```java
@Entity
public class Parent {
    
    @Id @GeneratedValue
    private Long id;
    
    @OneToMany(mappedBy = "parent", orphanRemoval = true)
    private List<Child> children = new ArrayList<>();
}

public void deleteChild(EntityManager em) {
    Parent parent = em.find(Parent.class, id);
    parent.getChildren().remove(0); // 자동으로 자식 엔티티 삭제
}

```

<br>


## 참고
[자바 ORM 표준 JPA 프로그래밍 - 김영한 저자](https://product.kyobobook.co.kr/detail/S000000935744)

2023-07-17

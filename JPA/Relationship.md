# 연관관계
    객체는 참조(주소)를 통해 관계를 맺고 테이블은 외래키를 통해 연관관계를 맺음

    따라서 JPA에서 연관관계는 객체의 참조와 테이블의 외래키를 매핑하는 것
<br>

### 객체의 연관관계
- 참조(주소)로 연관관계를 맺음
- 코드를 통해 단방향, 양방향을 설정할 수 있음
- 조회 시 참조(g.get())를 사용

```java
//객체 연관관계 설정
    Member member1 = new Member("member1", "회원1");
    Team team1 = new Team("team1", "팀1");
    
    member1.setTema(team1);
    
    // 조회
    Team findTeam = member1.getTeam();
```
### 위 코드처럼 객체는 참조를 사용해서 연관관계 탐색이 가능함, 이걸 객체그래프 탐색이라고 한다.
<br>

### 테이블 연관관계
- 외래키로 연관관계를 맺음
- 항상 양방향 연관관계
- 조회 시 JOIN을 사용
```mysql
SELECT T.*
FROM MEMBER M
JOIN TEAM T on M.TEAM_ID = T.TEAM_ID
WHERE M.MEMBER_ID = 'member1'
```
<br>

### 양방향관계는 서로 다른 단방향 관계 2개이다.

## 방향 (Direction)
    - 단방향: 두 개의 객체 중 한쪽만 참조하는 경우
    - 양방향: 양쪽 모두 서로 참조하는 것
    -> 방향은 객체관계에만 존재, 테이블 관계는 항상 양방향


<br>

### 1) 단방향 연관관계
<img src="https://github.com/MoMoon-LKH/GameInfo_Ver2/assets/66755342/375ec273-bb0e-452e-9559-b74fea9a6812" width="600"/> <br>

```java
// 위 사진을 코드로 옮기면
@Entity
public class Member {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "MEMBER_ID")
    private Long id;

    private String name;
    
    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;
}


@Entity
public class Team {
    
    @Id @GeneratedValue(startegy = GenerationType.IDENTITY)
    @Column(name = "TEAM_ID")
    private Long id;
    
    private String name;

/*    
    어노테이션 설명
    (1) @ManyToOne
    - 다대일(1:N) 매핑 어노테이션
    - 다중성을 나타내는데 필수적으로 사용

    (2) JoinColumn
    - 외래 키를 매핑할 때 사용
    - Column에서 지정한 name으로 매핑
  */  
}
```
#### 객체 연관관계 : Member.team
#### 테이블 연관관계: MEMBER.TEAM_ID
<br>


### 2) 양방향 연관관계
<img src="https://github.com/MoMoon-LKH/GameInfo_Ver2/assets/66755342/13b03f02-cb25-4345-b52d-60dc08ff7fe9" width="600">

```java
// 위 사진을 코드로 옮기면
@Entity
public class Member {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "MEMBER_ID")
    private Long id;

    private String name;
    
    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;
}


@Entity
public class Team {

    @Id @GeneratedValue(startegy = GenerationType.IDENTITY)
    @Column(name = "TEAM_ID")
    private Long id;
    
    private String name;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();

/*    
    어노테이션 설명
    (1) @OneToMany
    - 양방향 매핑일 때 사용
    - 반대쪽 매핑의 필드 이름을 값으로 주면됨
*/  
}
```

<br>

## 다중성 (Multiplicity)
    - 다대일(N:1), 일대다(1:N), 일대일(1:1), 다대다(N:M)
<br>

## 연관관계의 주인 (owner)
    - 양방향이라면 객체의 참조는 2개지만 외래 키는 하나
    -> 두 객체 연관관계 중 하나를 정해서 테이블의 외래키를 관리 = 연관관계의 주인
### 연관관계의 주인만이 데이터베이스 연관관계와 외래키를 관리(CUD) 가능
### 외래키를 가진 쪽이 연관관계의 주인 

- 객체 관점에선 양쪽 방향 모두 값을 입력해주는게 안전
```java
public void relationship_bidirectional() {
    
    member1.setTeam(team1); // member1 -> team1
    team1.setMembers().add(member1); // team1 -> member1
    em.persist(member1);
}
```

<br>

## 연관관계에 따른 사용방법
### 1) 저장
```java
    void save(){
    
        // 팀 저장
        Team team = new Team(1L, "팀1");
        em.persist(team);
        
        //회원 저장
        Member member = new Member(1L, "member1");
        member.setTeam(team); // 연관관계 설정 member -> team
        em.persist(member)
    }
```
<br>

### 2) 조회
#### (1) 객체 그래프 탐색
```java
    Member member = em.find(Member.class, "member1");
    Team team = member.getTeam();
```
<br>

#### (2) 객체지향 쿼리 사용 JPQL
```java
private void queryOfObject(EntityManager em) {
    
    String jpql = "select m from Member m join m.team t where t.name = :teamName";
    
    List<Member> resultList =  em.createQuery(jpql, Member.class)
        .setParameter("teamName", "팀1")
        .getResultList();
}
```
<br>

### 3) 수정
```java
private void update(EntityManager em){
    
    // 새로운 팀
    Team team2 = new Team("팀2");
    em.persist(team2);
    
    // 회원1 -> team2로 수정
    Member member = em.find(Member.class, "member1");
    member.setTeam(team2); // 수정
}

// 트랜잭션으로 커밋할 때 플러시 -> 변경감지를 통해 update
```

### 4) 제거
```java
Member member1 = em.find(Member.class, "member1");
member1.setTeam(null);
```
- 연관된 엔티티를 삭제하기 전 기존에 있던 연관관계를 모두 제거하고 삭제해야한다 <br>
  -> 삭제 안할 시 외래키 제약조건에 의해 데이터베이스에서 오류가 발생

<br>

## 참고
[자바 ORM 표준 JPA 프로그래밍 - 김영한 저자](https://product.kyobobook.co.kr/detail/S000000935744)

2023-07-14
# 프록시
### 실제 엔티티 대신에 조회를 지연시킬 수 있는 가짜객체

## 특징
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/a24a8ff8-fec8-4e59-9f5e-c62df2b021ad" width="600">

- 실제객체에 대한 참조를 보관
- 조회할 때 실제 엔티티 개체를 생성 
- 주로 지연 로딩일 때 많이 쓴다

## 프록시 초기화
<img src="https://github.com/MoMoon-LKH/TIL/assets/66755342/3bb07b65-545c-4f90-856e-003aeacc7ddf" width="600">

```java
class MemberProxy extends Member {
    
    Member target = null; // 실제 앤티티 참조
    
    public String getName() {
        if (target == null) {
            
            //2. 초기화 요청
            //3. DB 조회
            //4. 실제 엔티티 생성 및 참조 보관
        }
        
        return target.getName();
    }
}
```
- 프록시 객체를 통해서 실제 엔티티에 접근
<br>

## 프록시와 식별자의 관계
```java
Team team = em.getReference(Team.class , "team1"); // 식별자 보관
team.getId(); // 초기화 안됨
```

- 프록시 객체는 식별자 값을 가지고 있다 -> 즉 초기화 되지않는다. <br> 
  ㄴ @Access(AccessType.PROPERTIY))로 설정한 경우만
<br>


## 프록시 확인 방법
    PersistenceUnitUtil.isLoaded(Object object) 메소드를 통해 확인 가능
    ㄴ 초기화 X false, 초기화 true

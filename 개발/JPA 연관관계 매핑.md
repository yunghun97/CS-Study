# πΌ JPA μ°κ΄κ΄κ³ λ§€ν

### λ°©ν₯
- λ¨λ°©ν₯ : νμ -> ν
- μλ°©ν₯ : νμ -> ν, ν -> νμ  
> λ°©ν₯μ κ°μ²΄κ΄κ³μλ§ μ‘΄μ¬νκ³  νμ΄λΈ κ΄κ³λ ν­μ μλ°©ν₯μ΄λ€.
  
### λ€μ€μ±
- N:1, 1:N, 1:1, N:N
- νμ -> ν :  νμκ³Ό ν(λ€λμΌ) νκ³Ό νμ(μΌλλ€) 
  
### μ°κ΄κ΄κ³μ μ£ΌμΈ
κ°μ²΄λ₯Ό μλ°©ν₯ μ°κ΄κ΄κ³λ‘ λ§λ€λ©΄ μ°κ΄κ΄κ³μ μ£ΌμΈμ μ ν΄μΌνλ€.
  
## λ¨λ°©ν₯ μ°κ΄κ΄κ³
### μ°κ΄κ΄κ³ μ€μμ  (N:1) λ¨λ°©ν₯ κ΄κ³λ₯Ό κ°μ₯ λ¨Όμ  μ΄ν΄ν΄μΌ νλ€. μ§κΈλΆν° νμκ³Ό νμ κ΄κ³λ₯Ό ν΅ν΄ λ€λμΌ λ¨λ°©ν₯ κ΄κ³λ₯Ό μμλ³΄μ
- νμκ³Ό νμ΄ μλ€.
- νμμ νλμ νμλ§ μμλ  μ μλ€.
- νμκ³Ό νμ λ€λμΌ κ΄κ³μ΄λ€.
![image](https://user-images.githubusercontent.com/71022555/164226235-4b73dfaa-f9e2-4389-bdab-a4dea88de475.png) 
##### κ·Έλ¦Ό1
  
### κ·Έλ¦Ό μ€λͺ
- κ°μ²΄ μ°κ΄κ΄κ³
    - νμ κ°μ²΄λ Member.team νλ(λ©€λ² λ³μ)λ‘ ν κ°μ²΄μ μ°κ΄κ΄κ³λ₯Ό λ§Ίλλ€.
    - νμ κ°μ²΄μ ν κ°μ²΄λ λ¨λ°©ν₯ κ΄κ³μ΄λ€. νμμ Member.team νλλ₯Ό ν΅ν΄μ νμ μ μ μμ§λ§ λ°λλ‘ νμ νμμ μ μ μλ€. μλ₯Ό λ€μ΄ member -> teamμ μ‘°νλ member.getTeam()μΌλ‘ κ°λ₯νμ§λ§ λ°λ λ°©ν₯μΈ team -> memberλ₯Ό μ κ·Όνλ νλλ μλ€.
- νμ΄λΈ μ°κ΄κ΄κ³
    - νμ νμ΄λΈμ TEAM_ID μΈλ ν€λ‘ ν νμ΄λΈκ³Ό μ°κ΄κ΄κ³λ₯Ό λ§Ίλλ€.
    - νμ νμ΄λΈκ³Ό ν νμ΄λΈμ μλ°©ν₯ κ΄κ³μ΄λ€. νμ νμ΄λΈμ TEAM_ID μΈλ ν€λ₯Ό ν΅ν΄μ νμκ³Ό νμ μ‘°μΈν  μ μκ³  λ°λλ‘ νκ³Ό νμλ μ‘°μΈν  μ μλ€. μλ₯Ό λ€μ΄ MEMBER νμ΄λΈμ TEAM_ID μΈλ ν€ νλλ‘ MEMBER JOIN TEAMκ³Ό TEAM JOIN MEMEBER λ λ€ κ°λ₯νλ€.
- κ°μ²΄ μ°κ΄κ΄κ³μ νμ΄λΈ μ°κ΄κ΄κ³μ κ°μ₯ ν° μ°¨μ΄
> μ°Έμ‘°λ₯Ό ν΅ν μ°κ΄κ΄κ³λ μΈμ λ **λ¨λ°©ν₯**μ΄λ€. κ°μ²΄κ°μ μ°κ΄κ΄κ³λ₯Ό μλ°©ν₯μΌλ‘ λ§λ€κ³  μΆμΌλ©΄ λ°λμͺ½μλ νλλ₯Ό μΆκ°ν΄μ μ°Έμ‘°λ₯Ό λ³΄κ΄ν΄μΌ νλ€. κ²°κ΅­ μ°κ΄κ΄κ³λ₯Ό νλ λ λ§λ€μ΄μΌ νλ€.  μ΄λ₯Ό **μλ°©ν₯ μ°κ΄κ΄κ³λΌ νλ€.** μ ννκ²λ **μλ‘ λ€λ₯Έ λ¨ λ°©ν₯ κ΄κ³ 2κ°μ΄λ€.**
```java
// λ¨λ°©ν₯ μ°κ΄κ΄κ³
class A{
    B b;
}
class B{

}
// μλ°©ν₯ μ°κ΄κ΄κ³
class A{
    B b;
}
class B{
    A a;
}
```
### κ°μ²΄ μ°κ΄κ΄κ³ vs νμ΄λΈ μ°κ΄κ΄κ³
> κ°μ²΄λ **μ°Έμ‘°(μ£Όμ)**λ‘ μ°κ΄κ΄κ³λ₯Ό λ§Ίλλ€.  
νμ΄λΈμ **μΈλ ν€**λ‘ μ°κ΄κ΄κ³λ₯Ό λ§Ίλλ€.  
μ΄ λμ λΉμ·ν΄ λ³΄μ΄μ§λ§ κ°μ²΄λ μ°Έμ‘°(a.getB())λ₯Ό μ¬μ©νμ§λ§ νμ΄λΈμ **μ‘°μΈ**μ μ¬μ©νλ€.
- μ°Έμ‘°λ₯Ό μ¬μ©νλ κ°μ²΄μ μ°κ΄κ΄κ³λ λ¨λ°©ν₯μ΄λ€.
- μΈλν€λ₯Ό μ¬μ©νλ νμ΄λΈμ μ°κ΄κ΄κ³λ μλ°©ν₯μ΄λ€.

## π€  κ°μ²΄ κ΄κ³ λ§€ν
μμ κ·Έλ¦Ό1 κΈ°μ€ JPAλ₯Ό μ¬μ©νμ¬ λμ λ§€νν΄λ³΄μ.
```java
@Entity
public class Member{
    @Id
    @Column(name = "MEMBER_ID")
    private String id;
    private String username;

    // μ°κ΄κ΄κ³ λ§€ν
    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;

    // μ°κ΄κ΄κ³ μ€μ 
    public void setTeam(Team team){
        this.team = team;
    }
}

@Entity
public class Team{
    @Id
    @Coluimn(name ="TEAM_ID")
    private String id;
    
    private String name;
}
```
- κ°μ²΄ μ°κ΄κ΄κ³ : νμ κ°μ²΄μ Member.teamμ μ¬μ©
- νμ΄λΈ μ°κ΄κ΄κ³ : νμ νμ΄λΈμ MEMBER.TEAM_ID μΈλ ν€ μ»¬λΌ μ¬μ©
### μ΄λΈνμ΄μ μ€λͺ
> **@ManyToOne** : μ΄λ¦ κ·Έλλ‘ λ€λμΌ(N:1) κ΄κ³λΌλ λ§€ν μ λ³΄λ€. νμκ³Ό νμ λ€λμΌ κ΄κ³μ΄λ€. μ°κ΄κ΄κ³λ₯Ό λ§€νν  λ μ΄λ κ² λ€μ€μ±μ λνλ΄λ μ΄λΈνμ΄μμ νμλ‘ μ¬μ©ν΄μΌ νλ€.  
**@JoinColumn(name="TEAM_ID")** : μ‘°μΈ μ»¬λΌμ μΈλ ν€λ₯Ό λ§€νν  λ μ¬μ©νλ€. name μμ±μλ λ§€νν  μΈλ ν€ μ΄λ¦μ μ§μ νλ€. νμκ³Ό ν νμ΄λΈμ TEAM_ID μΈλ ν€λ‘ μ°κ΄κ΄κ³λ₯Ό λ§ΊμΌλ―λ‘ μ΄ κ°μ μ§μ νλ©΄ λλ€. μ΄ μ΄λΈνμ΄μμ μλ΅ν  μ μλ€.

### @JoinColumn
|μμ±|κΈ°λ₯|κΈ°λ³Έκ°|
|:---|:---|:---|
|name|λ§€νν  μΈλ ν€ μ΄λ¦|νλλͺ+_+μ°Έμ‘°νλ νμ΄λΈμ κΈ°λ³Έ ν€ μ»¬λΌ λͺ|
|referencedColumnName|μΈλ ν€κ° μ°Έμ‘°νλ λμ νμ΄λΈμ μ»¬λΌλͺ|μ°Έμ‘°νλ νμ΄λΈμ κΈ°λ³Έ ν€ μ»¬λΌλͺ|
|foreignKey(DDL)|μΈλ ν€ μ μ½μ‘°κ±΄μ μ§μ  μ§μ ν  μ μλ€. μ΄ μμ±μ νμ΄λΈμ μμ±ν  λλ§ μ¬μ©νλ€.||
|unique, nullable, insertable, updatable, columnDefinitaion, table|@Column μμ±κ³Ό κ°λ€.||

### @ManyToOne
|μμ±|κΈ°λ₯|κΈ°λ³Έκ°|
|:---|:---|:---|
|optional|falseλ‘ μ€μ νλ©΄ μ°κ΄λ μν°ν°κ° ν­μ μμ΄ν νλ€.|true|
|fetch|κΈλ‘λ² ν¨μΉ μ λ΅μ μ€μ νλ€.(N+1) λ¬Έμ |@ManyToOne=FetchType.EAGER @OneToMany=FetchType.LAZY|
|cascade|μμμ± μ μ΄ κΈ°λ₯μ μ¬μ©νλ€.||
|targetEntity|μ°κ΄λ μν°ν°μ νμ μ λ³΄λ₯Ό μ€μ νλ€. μ΄ κΈ°λ₯μ κ±°μ μ¬μ©νμ§ μλλ€. μ»¬λ μμ μ¬μ©ν΄λ μ λλ¦­μΌλ‘ νμμ λ³΄λ₯Ό μ μ μλ€.||
```java
// targetEntity μ€λͺ
// μ λλ¦­
@OneToMany
private List<Member> members;

@OneToMany(targetEntity=Member.class)
private List members;
```

## π μ°κ΄κ΄κ³ μ¬μ©
### μ μ₯
```java
public void test(){
    // ν1 μ μ₯
    Team team1 = new Team("team1", "1ν");
    em.persist(team1); // save()λ μ μ₯ν ID κ°μ returnνλ€. persistλ λ°ννμ΄ void

    //νμ μ μ₯
    Memeber member1 = new Member("member1", "νμ1");
    member1.setTeam(team); // μ°κ΄κ΄κ³ μ€μ  memeber1 -> 
    em.persist(member1);
    team1
}
```
### μ‘°ν
μ°κ΄κ΄κ³κ° μλ μν°ν°λ₯Ό μ‘°ννλ λ°©λ²μ ν¬κ² 2κ°μ§ μ΄λ€.
1. κ°μ²΄ κ·Έλν νμ(κ°μ²΄ μ°κ΄κ΄κ³λ₯Ό μ¬μ©ν μ‘°ν)
2. κ°μ²΄μ§ν₯ μΏΌλ¦¬ μ¬μ©(JPQL)

```java
// κ°μ²΄ κ·Έλν νμ
Member member = em.find(Member.class "member1");
Team team = member.getTeam();
```
```java
// κ°μ²΄μ§ν₯ μΏΌλ¦¬ μ¬μ©
private static void queryLogicJoin(EntityManager em) {
    String jpql = "select m from Member m join m.team t where" + "t.name=:teamName";

    List<Member> resultList = em.createQuery(jpql, Member.class).setParameter("teamName","ν1").getResultList();

    for(Member member : resultList){
        System.out.println("[query] memeber.userName="+member.getUsername());
    }
}
// κ²°κ³Ό
[query] member.username=νμ1
[query] member.username=νμ2

/* μ€ν κ³Όμ 
:teamNameλ νλΌλ―Έν°λ‘ λ°μΈλ©λ°λ λ¬Έλ²μ΄λ€.
select m from Member m join m.team t where t.name=:teamName

SELECT M.* FROM MEMBER MEMBER
INNER JOIN
    TEAM TEAM ON MEMBER.TEAM_ID = TEAM1_.ID
WHERE 
    TEAM1_.NAME="ν1"
*/
```

### μμ 
```java
private static void update(EntityManager em){
    Team team2 = new Team("team2", "ν2");
    em.persist(team2);

    Member member = em.find(Member.class, "member1");
    member.setTeam(team2);
}
// μμ μ em.update() κ°μ λ©μλκ° μλ€. λ¨μν λΆλ¬μ¨ μν°ν°μ κ°λ§ λ³κ²½ν΄λλ©΄ νΈλμ­μμ μ»€λ°ν  λ flushκ° μΌμ΄λλ©΄μ λ³κ²½ κ°μ§ κΈ°λ₯μ΄ μλνλ€. κ·Έλ¦¬κ³  λ³κ²½μ¬ν­μ λ°μ΄ν°λ² μ΄μ€μ μλμΌλ‘ λ°μνλ€. μ΄λ μ°κ΄κ΄κ³λ₯Ό μμ ν  λλ κ°λ€.
```
### μ°κ΄κ΄κ³ μ κ±°
```java
private static void deleteRelation(EntityManager em){
    Member member1 = em.find(Member.class, "member1");
    member1.setTeam(null);
}
```

### μ°κ΄λ μν°ν° γνμ 
μ°κ΄λ μν°ν°λ₯Ό μ­μ νλ €λ©΄ **κΈ°μ‘΄μ μλ μ°κ΄κ΄κ³λ₯Ό λ¨Όμ  μ κ±°νκ³  μ­μ ν΄μΌ νλ€.**
```java
member1.setTeam(null);
member2.setTeam(null);
em.remove(team);
```

## π μλ±‘ν₯ μ°κ΄κ΄κ³
μ§κΈκΉμ§λ νμμμ νμΌλ‘ μ κ·Όνλ λ€λμΌ λ¨λ±‘ν₯ λ§€νμ΄μλ€. μ΄λ²μλ νμμ νμμΌλ‘ μ κ·Όνλ κ΄κ³λ₯Ό μΆκ°ν΄λ³΄μ.
![image](https://user-images.githubusercontent.com/71022555/167242241-bf74de2c-062b-4428-8c1a-fee9a53edabc.png)  

### μλ°©ν₯ μ°κ΄κ΄κ³ λ§€ν
```java
// νμ μν°ν°λ κΈ°μ‘΄ κ·Έλλ‘ μ§ν

@Entity
public class Team{
    @Id
    @Coluimn(name ="TEAM_ID")
    private String id;
    
    private String name;

    // μΆκ°λ λΆλΆ
    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<Member>();
}
// μΌλλ€ κ΄κ³ μ΄λ―λ‘ ν μν°ν°μ List<Member> members λ¦¬μ€νΈ μ»¬λ μμ μΆκ°νμλ€. 
// mappedBy μμ±μ μλ°©ν₯ λ§€νμΌ λ μ¬μ©νλ λ°λμͺ½ λ§€νμ νλ μ΄λ¦μ κ°μΌλ‘ μ£Όλ©΄ λλ€. λ°λμͺ½ λ§€νμ΄ Team teamμ΄λ―λ‘ teamμ κ°μΌλ‘ μ£Όμλ€.
```
### μΌλλ€ μ»¬λ μ μ‘°ν
```java
public void biDirection(){
    Team team = en.find(Team.class "team1");
    List<Member> members = team.getMembers();

    for(Member member : members){
        System.out.println("member.username = "+member.getUsername());
    }
}
```
## π΅οΈββοΈ μ°κ΄κ΄κ³μ μ£ΌμΈ
@OneToManyλΏλ§μλλΌ **mappedBy**λ μ νμν κ²μΈκ°?
> κ°μ²΄μλ μλ°©ν₯ μ°κ΄κ΄κ³λΌλ κ²μ΄ μκΈ° λλ¬Έμ΄λ€.  
μ¦ **νμ΄λΈμ ν€ νλλ‘ λ νμ΄λΈμ μ°κ΄κ΄κ³λ₯Ό κ΄λ¦¬**νλ€.  
μ΄ μν©μμ μν°ν°λ₯Ό μλ°©ν₯μΌλ‘ λ§€ννλ©΄ νμ -> ν, ν -> νμ λ κ³³μμ μλ‘λ₯Ό μ°Έμ‘°λλ€. λ°λΌμ μ΄λ¬ν μν©μμ **μ°Έμ‘°λ λ μΈλν€λ νλ**λΌλ λ¬Έμ κ° λ°μνλ€.  
μ΄λ° μ°¨μ΄λ‘ μΈν΄ JPAμμλ **λ κ°μ²΄ μ°κ΄κ΄κ³ μ€ νλλ₯Ό μ ν΄μ νμ΄λΈμ μΈλν€λ₯Ό κ΄λ¦¬ν΄μΌ νλλ° μ΄κ²μ μ°κ΄κ΄κ³μ μ£ΌμΈ**μ΄λΌ νλ€.  

### μ°κ΄κ΄κ³ μ£ΌμΈ κ·μΉ
- **μ°κ΄κ΄κ³μ μ£ΌμΈλ§μ΄ λ°μ΄ν°λ² μ΄μ€ μ°κ΄κ΄κ³μ λ§€νλκ³  μΈλ ν€λ₯Ό κ΄λ¦¬(λ±λ‘, μμ , μ­μ )λ₯Ό ν  μ μλ€.** λ°λ©΄μ μ£ΌμΈμ΄ μλ μͺ½μ μ½κΈ°λ§ ν  μ μλ€.
- μ£ΌμΈμ **mappedBy**μμ±μ μ¬μ©νμ§ μλλ€. μ¦ mappedByκ° μλ μͺ½μ΄ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄λ€.
![image](https://user-images.githubusercontent.com/71022555/167242471-fdb3bf26-5f16-4e8a-a73f-ca79fdf6b6eb.png)  

```java
// νμ -> ν
class Member{
    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;
}
// ν -> νμ
class Team{
    @OneToMany
    private List<Member> members = new ArrayList<Member>();
}
```
> **μ°κ΄κ΄κ³μ μ£ΌμΈμ μ νλ€λ κ²μ μΈλ ν€ κ΄λ¦¬μλ₯Ό μ ννλ κ²μ΄λ€.**  
μ¬κΈ°μλ νμ νμ΄λΈμ μλ TEAM_ID μΈλ ν€λ₯Ό κ΄λ¦¬ν  κ΄λ¦¬μλ‘ μ ννμΌ νλ€.  
μ¬κΈ°μ νμ μν°ν°μ μλ Member.teamμ μ£ΌμΈμΌλ‘ μ ννλ©΄ μκΈ° νμ΄λΈμ μλ μΈλ ν€λ₯Ό κ΄λ¦¬νλ©΄ λλ€.  
νμ§λ§ ν μν°ν°μ μλ Team.membersλ₯Ό μ£ΌμΈμΌλ‘ μ ννλ©΄ λ¬Όλ¦¬μ μΌλ‘ μ ν λ€λ₯Έ νμ΄λΈμ μΈλ ν€λ₯Ό κ΄λ¦¬ν΄μΌνλ€. μ΄ κ²½μ° Team.membersκ° μλ Team μν°ν°λ TEAM νμ΄λΈμ λ§€νλμ΄ μλλ° κ΄λ¦¬ν΄μΌν  μΈλ ν€λ MEMBERνμ΄λΈμ μκΈ° λλ¬Έμ΄λ€.
### μ°κ΄κ΄κ²μ μ£ΌμΈμ μΈλ ν€κ° μλ κ³³
μ°κ΄κ΄κ³μ μ£ΌμΈμ νμ΄λΈμ μΈλ ν€κ° μλ κ³³μΌλ‘ μ ν΄μΌ νλ€.  
μ¬κΈ°μλ νμ νμ΄λΈμ μΈλν€λ₯Ό κ°μ§κ³  μμΌλ―λ‘ Member.teamμ΄ μ£ΌμΈμ΄ λλ€.

## μλ°©ν₯ μ°κ΄κ΄κ³ μ μ₯
```java
public void test(){
    // ν1 μ μ₯
    Team team1 = new Team("team1", "1ν");
    em.persist(team1); // save()λ μ μ₯ν ID κ°μ returnνλ€. persistλ λ°ννμ΄ void

    //νμ μ μ₯
    Memeber member1 = new Member("member1", "νμ1");
    member1.setTeam(team1); // μ°κ΄κ΄κ³ μ€μ  memeber1 -> team1
    em.persist(member1);
}
// μλ°©ν₯ μ°κ΄κ΄κ³λ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄ μΈλ ν€λ₯Ό κ΄λ¦¬νλ€. λ°λΌμ μ£ΌμΈμ΄ μλ λ°©ν₯μ κ°μ μ€μ νμ§ μμλ DBμ μΈλ ν€ κ°μ΄ μ μ μλ ₯λλ€.
team1.getMembers{}.add(member1); // λ¬΄μ λλ€.(μ°κ΄κ΄κ³μ μ£ΌμΈ X)

member1.setTeam(team1); //μ°κ΄κ΄κ³ μ€μ (μ°κ΄κ΄κ³ μ£ΌμΈ)

```

## β μλ°©ν₯ μ°κ΄κ΄κ³μ μ£Όμμ 
μ°κ΄κ΄κ³ μ£ΌμΈμλ κ°μ μλ ₯νμ§ μκ³  μ£ΌμΈμ΄ μλ κ³³μλ§ κ°μ μλ ₯νλ κ²μ΄λ€.

```java
// μ£ΌμΈμ΄ μλ κ³³μλ§ κ° μ€μ 
public void testSave(){
    // νμ 1 μ μ₯
    Member member1 = new Member("member1", "νμ1");
    em.persist(member1);

    // νμ 2 μ μ₯
    Member member2 = new Member("member2", "νμ2");
    em.persist(member2);

    Team team1 = new Team("team1", "ν1");
    // μ£ΌμΈμ΄ μλ κ³³μλ§ μ°κ΄κ΄κ³ μ€μ 
    team1.getMembers{}.add(member1);
    team1.getMembers{}.add(member2);
    
    em.persist(team1)
}
// κ²°κ³Ό μ‘°νμ
MEMBER_ID USERNAME TEAM_ID
member1   νμ1     null
member2   νμ2     null
```
μΈλ ν€ TEAM_IDμ team1μ΄ μλ null κ°μ΄ μλ ₯λμ΄ μλλ°, μ°κ΄κ΄κ³μ μ£ΌμΈμ΄ μλ Team.membersμλ§ κ°μ μ μ₯νκΈ° λλ¬Έμ΄λ€. μ¦ **μ°κ΄κ΄κ³μ μ£ΌμΈλ§μ΄ μΈλ ν€μ κ°μ λ³κ²½ν  μ μλ€.** μ΄ μ½λμμλ Member.teamμ μλ¬΄ κ°λ μλ ₯νμ§ μμλ€. λ°λΌμ TEAM_ID μΈλ ν€μ κ°λ nullμ΄ μ μ₯λλ€.

### μμν κ°μ²΄κΉμ§ κ³ λ €ν μλ°©ν₯ μ°κ΄κ΄κ³
JPAλ₯Ό μ¬μ©νμ§ μλ μμν κ°μ²΄ μνμμλ μμͺ½μ λ€ κ°μ μλ ₯ν΄μ£Όμ΄μΌ μ¬κ°ν λ¬Έμ κ° λ°μνμ§ μλλ€.
```java
public void testObject(){
    // ν1
    Team team1 = new Team("team1", "ν1");
    Member member1 = new Member("member1", "νμ1");
    Member member2 = new Member("member2", "νμ2");    

    member1.setTeam(team1);
    member2.setTeam(team1);

    List<Member> members = team1.getMembers();

    System.out.println(members.size());    
        
}
// κ²°κ³Όλ 0μ΄ μΆλ ₯λλ€.
// Member.teamμλ§ μ°κ΄κ΄κ³λ₯Ό μ μ₯νλ€.
team1.getMembers{}.add(member1); // μ²λΌ μμͺ½ λ€ κ΄κ³λ₯Ό μ€μ ν΄μΌ νλ€.
```

### λ λ€ κ³ λ €ν μ½λ
```java
public void testPerfect(){

    // ν1 μ μ₯
    Team team1 = new Team("team1", "ν1");
    em.persist(team1);
    
    Member member1 = new Member("member1", "νμ1");    

    // μλ°©ν₯ μ°κ΄κ΄κ³ μ€μ 
    member1.setTeam(team1); // member1 -> team1
    team1.getMembers{}.add(member1);  // team1 -> member1
    em.persist(member1);

    Member member2 = new Member("member2", "νμ2");  team1.getMembers{}.add(member2);
    em.persist(member2);  
}
```

### λ¦¬ν©ν λ§ ν μ½λ
```java
public class Memeber{
    private Team team;
    
    public void setTeam(Team team){
        this.team = team;
        this.getMembers().add(this);
    }
}

Member.setTeam(team) // λ©μλ νλλ‘ μλ°©ν₯ κ΄κ³λ₯Ό λͺ¨λ μ€μ νλλ‘ λ³κ²½
```

### νΈμ λ©μλ μμ± μ μ μ μ¬ν­
```java
member1.setTeam(team1); 
member2.setTeam(team2); 
Member findMember = team1.getMember(); // member1μ΄ μ¬μ ν μ‘°νλλ€.

// κΈ°μ‘΄μ team2λ‘ λ³κ²½ν  λ team1 -> member1 κ΄κ³λ₯Ό μ κ±°νμ§ μμμ member1μ΄ μ¬μ ν κ²μλλ€.

public void setTeam(Team team){
    if(this.team != null){
        this.team.getMembers().remove(this);
    }

    this.team = team;
    this.getMembers().add(this);
}
```
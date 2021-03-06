# π JPA λ€μν μ°κ΄κ΄κ³ λ§€ν

## π λ€λμΌ
> λ€λμΌ κ΄κ³μ λ°λ λ°©ν₯μ ν­μ μΌλλ€ κ΄κ³κ³  μΌλλ€ κ΄κ³μ λ°λ λ°©ν₯μ ν­μ λ€λμΌ κ΄κ³μ΄λ€.  
> **λ°μ΄ν°λ² μ΄μ€ νμ΄λΈμ 1:N κ΄κ³μμ μΈλ ν€λ ν­μ N μͺ½μ μλ€.**  
> λ°λΌμ κ°μ²΄ μλ°©ν₯ κ΄κ³μμ μ°κ΄κ΄κ³μ μ£ΌμΈμ ν­μ N(λ€)μͺ½μ΄λ€. νμ(N)κ³Ό ν(1)μ΄ μμΌλ©΄ νμμͺ½μ΄ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄λ€.

### λ€λμΌ λ¨λ°©ν₯[N:1]  
![image](https://user-images.githubusercontent.com/71022555/174429565-befdc610-05b1-4d51-9373-75e8412c892a.png)  

```java
// νμ
@Entity
public class Member{
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    private String username;

    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;

}
// ν
@Entity
public class Team{
    @Id @GeneratedValue
    @Column(name="TEAM_ID")
    private Long id;
    private String naeme;
}
```

### λ€λμΌ μλ°©ν₯[N:1, 1:N]  
![image](https://user-images.githubusercontent.com/71022555/174435503-9915c2b1-e659-46e5-b328-f6d0b1ea432d.png)  
```java
// νμ
@Entity
public class Member{
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    private String username;

    @ManyToOne
    @JoinColumn(name="TEAM_ID")
    private Team team;
    public void setTeam(Team team){
        this.team = team;
        // λ¬΄νλ£¨ν λΉ μ§μ§ μλλ‘ μ²΄ν¬
        if(!team.getMembers.contains(this)){
            team.getMembers.add(this);
        }
    }
}

// ν
@Entity
public class Team{
    @Id @GeneratedValue
    @Column(name="TEAM_ID")
    private Long id;
    private String name;

    @OneToMany(mappedBy="team") // mappedByκ° μμΌλ―λ‘ μ£ΌμΈμ΄ μλ
    private List<Member> members = new ArrayList<Member>();

    public void addMember(Member member){
        this.members.add(member);
        // λ¬΄νλ£¨ν λΉ μ§μ§ μλλ‘ μ²΄ν¬
        if(member.getTeam() != this){
            member.setTeam(this);
        }
    }
}
```
- μλ°©ν₯μ μΈλ ν€κ° μλ μͺ½μ΄ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄λ€.
> μΌλλ€μ λ€λμΌ μ°κ΄κ΄κ³λ ν­μ λ€(N)μ μΈλν€κ° μλ€. μ¬κΈ°μλ λ€μͺ½μΈ Member νμ΄λΈμ΄ μΈλ ν€λ₯Ό κ°μ§κ³  μμΌλ―λ‘ Member.teamμ΄ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄λ€. JPAλ μΈλ ν€λ₯Ό κ΄λ¦¬ν  λ μ°κ΄κ΄κ³μ μ£ΌμΈλ§ μ¬μ©νλ€. μ£ΌμΈμ μλ Team.membersλ μ‘°νλ₯Ό μν JPQLμ΄λ κ°μ²΄ κ·Έλνλ₯Ό νμν  λ μ¬μ©νλ€.
- μλ°©ν₯ μ°κ΄κ΄κ³λ ν­μ μλ‘λ₯Ό μ°Έμ‘°ν΄μΌ νλ€.
> μ΄λ ν μͺ½λ§ μ°Έμ‘°νλ©΄ μλ°©ν₯ μ°κ΄κ΄κ³κ° μ±λ¦½νμ§ μλλ€. ν­μ μλ‘ μ°Έμ‘°νκ² νλ €λ©΄ μ°κ΄κ΄κ³ νΈμ λ©μλλ₯Ό μμ±νλ κ²μ΄ μ’μλ° νμμ setTeam(), νμ addMember()κ° μ΄λ° νΈμ λ©μλλ€μ΄λ€. νΈμ λ©μλλ μμͺ½μ λ€ μμ±ν  μλ μλλ€. μμͺ½ μμ± μ λ¬΄νλ£¨νμ λΉ μ§μ§ μλλ‘ κ²μ¬ν΄μΌ νλ€.
## π μΌλλ€
- λ€λμΌ κ΄κ³μ λ°λ λ°©ν₯μ΄λ€. μΌλλ€ κ΄κ³λ μν°ν°λ₯Ό νλ μ΄μ μ°Έμ‘°ν  μ μμΌλ―λ‘ μλ° μ»¬λ μμΈ List, Set, Mapμ€μ νλλ₯Ό μ¬μ©ν΄μΌ νλ€.
### μΌλλ€ λ¨λ°©ν₯[1:N]
- νλμ νμ μ¬λ¬μ νμμ μ°Έμ‘°ν  μ μλλ° μ΄λ° κ΄κ³λ₯Ό μΌλλ€ κ΄κ³λΌ νλ€. 
- νμ νμλ€μ μ°Έμ‘°νμ§λ§ λ°λλ‘ νμμ νμ μ°Έμ‘°νμ§ μμΌλ©΄ λμ κ΄κ³λ λ¨λ°©ν₯μ΄λ€.  

![image](https://user-images.githubusercontent.com/71022555/174436156-a6f100e9-5fa9-460c-b63c-5acfab350b59.png)  

> μΌλλ€ λ¨λ°©ν₯ κ΄κ³λ ν μν°ν°μ Team.membersλ‘ νμ νμ΄λΈμ TEAM_ID μΈλ ν€λ₯Ό κ΄λ¦¬νλ€. λ³΄ν΅ μμ μ΄ λ§€νν νμ΄λΈμ μΈλ ν€λ₯Ό κ΄λ¦¬νλλ° μ΄ λ§€νμ λ°λμͺ½ νμ΄λΈμ μλ μΈλ ν€λ₯Ό κ΄λ¦¬νλ€.  
> κ·Έλ΄ μ λ°μ μλ κ²μ΄ μΌλλ€ κ΄κ³μμ μΈλ ν€λ ν­μ λ€μͺ½ νμ΄λΈμ μλ€. νμ§λ§ λ€μͺ½μΈ Member μν°ν°μλ μΈλ ν€λ₯Ό λ§€νν  μ μλ μ°Έμ‘° νλκ° μλ€. λμ  λ°λμͺ½μΈ TEAMμλ§ μ°Έμ‘° νλμΈ membersκ° μλ€. λ°λΌμ λ°λνΈ νμ΄λΈμ μΈλ ν€λ₯Ό κ΄λ¦¬νλ νΉμ΄ν λͺ¨μ΅μ΄ λνλλ€.
```java
// ν
@Entity
public class Team{
    @Id @GeneratedValue
    @Column(name="TEAM_ID")
    private Long id;
    private String name;

    @OneToMany
    @JoinColumn(name="TEAM_ID") // Member νμ΄λΈμ TEAM_ID(FK)
    private List<Member> members = new ArrayList<Member>();
}

// νμ
@Entity
public class Member{
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    private String username;
    
}
```
> μΌλλ€ λ¨λ°©ν₯ κ΄κ³λ₯Ό λ§€νν  λλ @JoinColumnμ λͺμν΄μΌ νλ€. κ·Έλ μ§ μμΌλ©΄ JPAλ μ°κ²° νμ΄λΈμ μ€κ°μ λκ³  μ°κ΄κ΄κ³λ₯Ό κ΄λ¦¬νλ μ‘°μΈ νμ΄λΈ μ λ΅μ κΈ°λ³Έμ μ¬μ©ν΄μ λ§€ννλ€.

### μΌλλ€ λ¨λ°©ν₯ λ§€νμ λ¨μ 
- λ§€νν κ°μ²΄κ° κ΄λ¦¬νλ μΈλ ν€κ° λ€λ₯Έ νμ΄λΈμ μλ€λ μ μ΄λ€.
- λ³ΈμΈ νμ΄λΈμ μΈλ ν€κ° μμΌλ©΄ μν°ν°μ μ μ₯κ³Ό μ°κ΄κ΄κ³ μ²λ¦¬λ₯Ό INSERT SQL ν λ²μΌλ‘ λλΌ μ μμ§λ§ λ€λ₯Έ νμ΄λΈμ μλ ν€κ° μμΌλ©΄ μ°κ΄κ΄κ³ μ²λ¦¬λ₯Ό μν UPDATE SQLμ μΆκ°λ‘ μ€νν΄μΌ νλ€.
```java
public void saveTest(){
    Member member1 = new Member("member1");
    Member member2 = new Member("member2");

    Team team1 = new Team("team1");
    team1.getMembers().add(member1);
    team1.getMembers().add(member2);

    em.persist(member1); // INSERT member1
    em.persist(member2); // INSERT member2
    em.persist(team1) // INSERT team1,  UPPDATE=member1.fk, UPDATE=member2.fk
}
```
```sql
-- μ μ½λ SQL μ€ν κ²°κ³Ό
insert into Member(MEMBER_ID, username) values(null, ?)
insert into Member(MEMBER_ID, username) values (null, ?)
insert into Team(TEAM_ID, name) values(null, ?)
update Member set TEAM_ID=? where MEMBER_ID=?
update Member set TEAM_ID=? where MEMBER_ID=?
```
- Member μν°ν°λ Team μν°ν°λ₯Ό λͺ¨λ₯Έλ€. κ·Έλ¦¬κ³  μ°κ΄κ΄κ³μ λν μ λ³΄λ Team μν°ν°μ membersκ° κ΄λ¦¬νλ€. λ°λΌμ Member μν°ν°λ₯Ό μ μ₯ν  λλ MEMBER νμ΄λΈμ TEAM_ID μΈλ ν€μ μλ¬΄ κ°λ μ μ₯λμ§ μλλ€. λμ  TEAM μν°ν°λ₯Ό μ μ₯ν  λ TEAM.membersμ μ°Έμ‘° κ°μ νμΈν΄μ νμ νμ΄λΈμ΄ TEAM_IDλ₯Ό μλ°μ΄νΈ νλ€.
> μΌλλ€ λ¨λ°©ν₯ λ§€νλ³΄λ€λ λ€λμΌ μλ°©ν₯ λ§€νμ μ¬μ©νμ.  
  
> μΌλλ€ λ¨λ°©ν₯ λ§€νμ μ¬μ©νλ©΄ μν°ν°λ₯Ό λ§€νν νμ΄λΈμ΄ μλ λ€λ₯Έ νμ΄λΈμ μΈλ ν€λ₯Ό κ΄λ¦¬ν΄μΌ νλ€. μ΄λ μ±λ₯λΏλ§ μλλΌ κ΄λ¦¬λ λΆλ΄μ€λ½λ€. μ΄λ₯Ό ν΄κ²°νλ μ¬μ΄ λ°©λ²μ λ€λμΌ μλ°©ν₯ λ§€νμ μ¬μ©νλ κ²μ΄λ€. λ€λμΌ μλ°©ν₯ λ§€νμ κ΄λ¦¬ν΄μΌ νλ μΈλ ν€κ° λ³ΈμΈ νμ΄λΈμ μλ€. 

### μΌλλ€ μλ°©ν₯[1:N, N:1]
- μΌλλ€ μλ°©ν₯ λ§€νμ μ‘΄μ¬νμ§ μλλ€.
- λμ° λ€λμΌ μλ°©ν₯ λ§€νμ μ¬μ©ν΄μΌ νλ€.(λκ°μ λ§μ΄λ€. μ¬κΈ°μλ μΌμͺ½μ μ°κ΄κ΄κ³μ μ£ΌμΈμΌλ‘ κ°μ ν΄μ λΆλ₯ ex) λ€λμΌμ΄λ©΄ λ€(N)μ΄ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄λ€.)
- **μ νν λ§νμλ©΄ μλ°©ν₯ λ§€νμμ @OneToManyλ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄ λ  μ μλ€.** 
- κ΄κ³ν λ°μ΄ν°λ² μ΄μ€μ νΉμ± μ λ€λμΌ κ΄κ³λ ν­μ λ€(N)μͺ½μ μΈλ ν€κ° μλ€. 
- λ°λΌμ @OneToMany, @ManyToOne λ μ€μ μ°κ΄κ΄κ³μ μ£ΌμΈμ ν­μ λ€ μͺ½μΈ @ManyToOneμ μ¬μ©ν κ³³μ΄λ€. μ΄λ° μ΄μ λ‘ @ManyToOneμλ mappedBy μμ±μ΄ μλ€.
- μμ ν λΆκ°λ₯ν κ²μ μλλ€. μΌλλ€ λ¨λ°©ν₯ λ§€ν λ°λνΈμ κ°μ μΈλ ν€λ₯Ό μ¬μ©νλ λ€λμΌ λ¨λ±‘ν­ λ§€νμ μ½κΈ° μ μ©μΌλ‘ νλ μΆκ°νλ©΄ λλ€.  

![image](https://user-images.githubusercontent.com/71022555/174436649-568afeb9-8946-4de2-8c40-e06e93052d4d.png)  
  
```java
@Entity
public class Team{
    @Id @GeneratedValue
    @Column(name="TEAM_ID")
    private Long id;
    private String name;

    @OneToMany
    @JoinColumn(name="TEAM_ID")
    private List<Member> members = new ArrayList<Member>();
}

// νμ
@Entity
public class Member{
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    private String username;
    
    @ManyToOne
    @JoinColumn(name="TEAM_ID", insertable = false, updatable = false)
    private Team team;
}
```
- λ€λμΌ μͺ½μ insertable updatable = false μ€μ μ μΆκ°ν΄μ μ½κΈ°λ§ κ°λ₯νκ²
- μΌλλ€ μλ°©ν₯ λ§€νμ΄λΌκΈ° λ³΄λ€λ μΌλλ€ λ¨λ°©ν₯ λ§€ν λ°λνΈμ λ€λμΌ λ¨λ±‘ν₯ λ§€νμ μ½κΈ° μ μ©μΌλ‘ μΆκ°ν΄μ μΌλλ€ μλ°©ν₯μ²λΌ λ³΄μ΄λλ‘ νλ λ°©λ²μ΄λ€.
- μΌλλ€ λ¨λ°©ν₯ λ§€νμ΄ κ°μ§λ λ¨μ μ κ·Έλλ‘ κ°μ§λ€.
- λ  μ μμΌλ©΄ λ€λμΌ μλ±‘ν­ λ§€νμ μ¬μ©νμ

## π μΌλμΌ[1:1]
- μμͺ½μ΄ μλ‘ νλμ κ΄κ³λ§ κ°μ§λ€.
- νμμ νλμ μ¬λ¬Όν¨λ§ μ¬μ©νκ³  μ¬λ¬Όν¨λ λ§μ°¬κ°μ§
- λ€μκ³Ό κ°μ νΉμ§μ΄ μλ€.
    - μΌλμΌ κ΄κ³λ κ·Έ λ°λλ μΌλμΌ κ΄κ³μ΄λ€.
    - νμ΄λΈ κ΄κ³μμ μΌλλ€, λ€λμΌμ ν­μ λ€(N)μͺ½μ΄ μΈλ ν€λ₯Ό κ°μ§λ€. λ°λ©΄μ μΌλμΌ κ΄κ³λ μ΄λ κ³³μ΄λ μΈλ ν€λ₯Ό κ°μ§ μ μλ€.
- μ£Ό νμ΄λΈμ μλ ν€
    - μ£Ό νμ΄λΈμ μΈλ ν€λ₯Ό λκ³  λμ νμ΄λΈμ μ°Έμ‘°νλ€. μΈλ ν€λ₯Ό κ°μ²΄ μ°Έμ‘°μ λΉμ·νκ² μ¬μ©ν  μ μμ΄μ κ°μ²΄μ§ν₯ κ°λ°μλ€μ΄ μ νΈνλ€. μ΄ λ°©λ²μ **μ£Ό νμ΄λΈμ΄ μΈλ ν€λ₯Ό κ°μ§κ³  μμΌλ―λ‘ μ£Ό νμ΄λΈλ§ νμΈν΄λ λμ νμ΄λΈκ³Ό μ°κ΄κ΄κ³κ° μλμ§ μ μ μλ€.
- λμ νμ΄λΈμ μΈλ ν€
    - μ ν΅μ μΈ DB κ°λ°μλ€μ λ³΄ν΅ λμ νμ΄λΈμ μΈλ ν€λ₯Ό λλ κ²μ μ νΈνλ€. μ΄ λ°©λ²μ μ₯μ μ νμ΄λΈ κ΄κ³λ₯Ό μΌλμΌμμ μΌλλ€λ‘ λ³κ²½ν  λ νμ΄λΈ κ΅¬μ‘°λ₯Ό κ·Έλλ‘ μ μ§ν  μ μλ€.

### μ£Ό νμ΄λΈμ μΈλ ν€
- κ°μ²΄μ§ν₯ κ°λ°μλ€μ΄ μ νΈνλ€.
- JPAλ μ£Ό νμ΄λΈμ μΈλ ν€κ° μμΌλ©΄ μ’ λ νΈλ¦¬νκ² λ§€νν  μ μλ€.
### λ¨λ°©ν₯
- MEMBERκ° μ£Ό νμ΄λΈ, LOCKERλ λμ νμ΄λΈ  
![image](https://user-images.githubusercontent.com/71022555/174436954-409026dc-22cb-4190-ad6d-009996c9d65a.png)  
```java
@Entity
public class Member{
    @Id @GeneratedValue
    @Column(name="MEMBER_ID")
    private Long id;

    private String username;

    @OneToOne
    @JoinColumn(name="LOCKER_ID")
    private Locker locker;
}
@Entity
public class Locker{
    @Id @GeneratedValue
    @Column(name="LOCKER_ID")
    private Long id;

    private String name;
}
```
- 1:1 κ΄κ³μ΄λ―λ‘ @OneToOne μ¬μ©
### μλ°©ν₯  

![image](https://user-images.githubusercontent.com/71022555/174437026-a13fbb59-4657-4de1-8050-f3f477b4d70c.png)  


```java
@Entity
public class Member{
    @Id @GeneratedValue
    @Column(name="MEMBER_ID")
    private Long id;

    private String username;

    @OneToOne
    @JoinColumn(name="LOCKER_ID")
    private Locker locker;
}
@Entity
public class Locker{
    @Id @GeneratedValue
    @Column(name="LOCKER_ID")
    private Long id;

    private String name;
    @OneToOne(mappedBy="locker")
    private Member member;
}
```
- Member.lockerκ° μ°κ΄κ΄κ³μ μ£ΌμΈμ΄λ€.
- Locker.memberλ mappedByλ₯Ό ν΅ν΄ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄ μλλΌκ³  μ€μ νλ€.

### λμ νμ΄λΈμ μΈλ ν€
#### λ¨λ°©ν₯  
![image](https://user-images.githubusercontent.com/71022555/174437109-4682c997-0c87-45b7-94b4-fefadeb99a87.png)  
  
- κ·Έλ¦Όμ λ³΄λ©΄ λμ νμ΄λΈμ μΈλ ν€κ° μλ λ¨λ°©ν₯ κ΄κ³λ JPAμμ μ§μνμ§ μλλ€. κ·Έλ¦¬κ³  μ΄λ° λͺ¨μμΌλ‘ λ§€νν  μ μλ λ°©λ²λ μλ€.
- μ΄λλ λ¨λ°©ν₯ κ΄κ³λ₯Ό Lockerμμ Member λ°©ν₯μΌλ‘ μμ νκ±°λ, μλ°©ν₯ κ΄κ³λ‘ λ§λ€κ³  Lockerλ₯Ό μ°κ΄κ΄κ³μ μ£ΌμΈμΌλ‘ μ€μ ν΄μΌ νλ€.
- JPA2.0 λΆν° μΌλλ€ λ¨λ°©ν₯ κ΄κ³μμ λμ νμ΄λΈμ μΈλ ν€κ° μλ λ§€νμ νμ©νλ€. νμ§λ§ μΌλμΌ λ¨λ°©ν₯μ μ΄λ° λ§€νμ νμ©νμ§ μλλ€.
  
#### μλ°©ν₯  
![image](https://user-images.githubusercontent.com/71022555/174437166-c5119f90-6f37-4f7a-af4f-ecc9183ef70f.png)  

```java
@Entity
public class Member{
    @Id @GeneratedValue
    @Column(name="MEMBER_ID")
    private Long id;

    private String username;

    @OneToOne(mappedBy="member")
    private Locker locker;
}
@Entity
public class Locker{
    @Id @GeneratedValue
    @Column(name="LOCKER_ID")
    private Long id;

    private String name;

    @OneToOne
    @JoinColumn(name="MEMBER_ID")
    private Member member;
}
```
  
> νλ‘μλ₯Ό μ¬μ©ν  λ μΈλ ν€λ₯Ό κ΄λ¦¬νμ§ μλ μΌλμΌ κ΄κ³λ μ§μ° λ‘λ©μΌλ‘ μ€μ ν΄λ μ¦μ λ‘λ©λλ€. μλ₯Ό λ€μ΄ Locker.memberλ μ§μ°λ‘λ© ν  μ μμ§λ§ Member.lockerλ μ§μ°λ‘λ©μΌλ‘ μ€μ ν΄λ μ¦μ λ‘λ©λλ€. μ΄κ²μ νλ‘μ νκ³ λλ¬Έμ λ°μνλ λ¬Έμ λ‘ **bytecode instrumentationμ μ¬μ©νλ©΄ ν΄κ²°ν  μ μλ€.**
  
## πΏ λ€λλ€[N:N]
- κ΄κ³ν λ°μ΄ν° λ² μ΄μ€λ μ κ·νλ νμ΄λΈ 2κ°λ‘ λ€λλ€ κ΄κ³λ₯Ό **ννν  μ μλ€.**
- λ³΄ν΅ λ€λλ€ κ΄κ³λ₯Ό μΌλλ€, λ€λμΌ κ΄κ³λ‘ νμ΄λ΄λ μ°κ²° νμ΄λΈμ μ¬μ©νλ€.  
  
![image](https://user-images.githubusercontent.com/71022555/174437407-f37ca950-70ae-4e99-8079-469acaf2ac88.png)  

- μλ κ·Έλ¦Όμ²λΌ μ€κ°μ μ°κ²° νμ΄λΈμ μΆκ°ν΄μΌνλ€.
- Member_Product μ°κ²° νμ΄λΈμ μΆκ°ν΄μ λ€λλ€ κ΄κ³λ₯Ό μΌλλ€, λ€λμΌ κ΄κ³λ‘ νμ΄λΌ μ μλ€.
- μ΄ μ°κ²° νμ΄λΈμ νμμ΄ μ£Όλ¬Έν μνμ λνλΈλ€.  

![image](https://user-images.githubusercontent.com/71022555/174437434-d85b4d05-0ae2-4914-8d5e-d68773769eef.png)  
  
> κ·Έλ°λ° κ°μ²΄λ νμ΄λΈκ³Ό λ€λ₯΄κ² κ°μ²΄ 2κ°λ‘ λ€λλ€ κ΄κ³λ₯Ό λ§λ€ μ μλ€. μλ₯Ό λ€μ΄ νμκ³Ό μν κ°μ²΄λ μ¬λ‘ μ»¬λ μμ μ¬μ©ν΄μ¬ μ°Έμ‘°νλ©΄ λλ€. @ManyToManyλ₯Ό μ¬μ©  

![image](https://user-images.githubusercontent.com/71022555/174437527-37aa76b1-335c-493a-862d-7fbc827bd9db.png)  
  
```java
// λ€λλ€ λ¨λ°©ν₯ νμ
@Entity
public class Member{
    @Id @Column(name="MEMBER_ID")
    private String id;
    private String username;

    @ManyToMany
    @JoinTable(name="MEMBER_PRODUCT", joinColumns = @JoinColumn(name="MEMBER_ID"),
    inverseJoinColumns = @JoinColumn(name="PRODUT_ID"))
    private List<Product> products = new ArrayList<Product>();

}
//  λ€λλ€ λ¨λ°©ν₯ μν
@Entity
public class Product{
    @Id @Column(name="PRODUCT_ID")
    private String id;
    private String name;
}
```
- νμ μν°ν°μ μν μν°ν°λ₯Ό @ManyToManyλ‘ λ§€ννλ€. **μ€μν μ μ @ManyToManyμ @JoinTableμ μ¬μ©ν΄μ μ°κ²° νμ΄λΈμ λ°λ‘ λ§€νν κ²μ΄λ€. λ°λΌμ νμκ³Ό μνμ μ°κ²°νλ νμ_μν(Member_Product) μν°ν° μμ΄ λ§€νμ μλ£ν  μ μλ€.**
- μ°κ²° νμ΄λΈμ λ§€ννλ **@JoinTableμ μμ±**
    - **@JoinTable.name** : μ°κ²° νμ΄λΈμ μ§μ νλ€. μ¬κΈ°μλ MEMBER_PRODUCT νμ΄λΈμ μ ν
    - **@JoinTable.joinColumns** : νμ¬ λ°©ν₯μΈ νμκ³Ό λ§€νν  μ‘°μΈ μ»¬λΌ μ λ³΄λ₯Ό μ§μ νλ€. MEMBER_IDλ‘ μ§μ 
    - **@JoinTable.inserseJoinColumns** : λ°λ λ°©ν₯μΈ μνκ³Ό λ§€νν  μ‘°μΈ μ»¬λΌ μ λ³΄λ₯Ό μ§μ νλ€. PRODUCT_IDλ‘ μ§μ νλ€.
- MEMBER_PRODUCT νμ΄λΈμ λ€λλ€ κ΄κ³λ₯Ό μΌλλ€, λ€λμΌ κ΄κ³λ‘ νμ΄λ΄κ° μν μ°κ²° νμ΄λΈμΌ λΏμ΄λ€. @ManyToManyλ‘ λ§€νν λλΆμ λ€λλ€ κ΄κ³λ₯Ό μ¬μ©ν  λλ μ΄ μ°κ²° νμ΄λΈμ μ κ²½ μ°μ§ μλ€λ λλ€.
  
```java
//  λ€λλ€ κ΄κ³λ₯Ό μ μ₯
public void save(){
    Product productA = new Product();
    productA.setId("productA");
    productA.setName("μνA");
    em.persist(productA);

    Member member1 = new Member();
    member1.setId("member1");
    member1.setUsername("νμ1");
    member1.getProducts().add(ProductA); // μ°κ΄κ΄κ³ μ€μ 
    em.empersist(member1);
}
```
```sql
-- κ²°κ³Ό
INSERT INTO PRODUCT ...
INSERT INTO MEMBER ...
INSERT INTO MEMBER_PRODUCT ...
```
```java
//  λ€λλ€ νμ
public void find(){
    Member member = em.find(Member.class, "member1");
    List<Product> products = nmember.getProducts(); // κ°μ²΄ κ·Έλν νμ
    for(Product product : products){
        System.out.println("product.name = " + product.getName());
    }
}
```
```sql
-- member.getProducts()
SELECT * FROM MEMBER_PRODUCT MP
INNER JOIN PRODUCT P ON MP.PRODUCT_ID=P.PRODUCT_ID
WHERE MP.MEMBER_ID =?
```
- SQLμ λ³΄λ©΄ MEMBER_PRODUCTμ μν νμ΄λΈμ μ‘°μΈν΄μ μ°κ΄λ μνμ μ‘°ννλ€.
- λ³΅μ‘ν λ€λλ€ κ΄κ³λ₯Ό μ νλ¦¬μΌμ΄μμμλ μμ£Ό λ¨μνκ² μ¬μ©ν  μ μλ€.

### λ€λλ€ μλ°©ν₯
- μ­λ°©ν₯λ @ManyToManyλ₯Ό μ¬μ©νλ€. κ·Έλ¦¬κ³  μνλ κ³³μ mappedByλ‘ μ°κ΄κ΄κ³μ μ£ΌμΈμ μ§μ νλ€.
```java
@Entity
public class Product{
    @Id
    private String id;
    @ManyToMany(mappedBy="products") // μ­λ°©ν₯ μΆκ°
    private List<Member> members;
}
```
> λ€λλ€μ μλ°©ν₯ μ°κ΄κ΄κ³λ λ€μμ²λΌ μ€μ νλ©΄ λλ€.  
> **member.getProducts().add(product);**  
> **product.getMembers().add(member);**
- μλ°©ν₯ μ°κ΄κ΄κ³λ μ°κ΄κ΄κ³ νΈμ λ©μλλ₯Ό μΆκ°ν΄μ κ΄λ¦¬νλ κ²μ΄ νΈλ¦¬νλ€.
```java
public void addProduct(Product product){
    products.add(product);
    product.getMembers().add(this);
}
```
#### μ­λ°©ν₯ νμ
```java
public void findInverse(){
    Product product = em.find(Product.class, "productA");
    Lsit<Member> members = product.getMembers();
    for(Member member : members){
        System.out.println("member = "+member.getUsername());
    }
}
```

## π₯ λ€λλ€ : λ§€νμ νκ³μ κ·Ήλ³΅, μ°κ²° μν°ν° μ¬μ©
- @ManyToManyλ₯Ό μ€λ¬΄μμ μ¬μ©νκΈ°μλ νκ³κ° μλ€.
- ex) νμμ΄ μνμ μ£Όλ¬Ένλ©΄ μ°κ²° νμ΄λΈμ λ¨μν μ£Όλ¬Έν νμ μμ΄λμ μν μμ΄λλ§ λ΄κ³  λλμ§ μλλ€.
- λ³΄ν΅μ μ°κ²° νμ΄λΈμ μ£Όλ¬Έ μλ μ»¬λΌμ΄λ μ£Όλ¬Έν λ μ§ κ°μ μ»¬λΌμ΄ λ νμνλ€.  
  
![image](https://user-images.githubusercontent.com/71022555/174438183-dbcb29d6-7da6-4054-b8a4-40bd2efae362.png)  
  
- μ΄λ κ² μ»¬λΌμ μΆκ°νλ©΄ @ManyToManyλ₯Ό μ¬μ©ν  μ μλ€.
  
![image](https://user-images.githubusercontent.com/71022555/174601057-3bea4ac8-7cd2-43e9-a466-9f78846ac43b.png)
  
- μ°κ²° νμ΄λΈμ λ§€ννλ μν°ν°λ₯Ό λ§λ€κ³  μ΄κ³³μ μΆκ°ν μ»¬λΌλ€μ λ§€νν΄μΌ νλ€.
- νμ΄λΈμ κ΄κ³λ νμ΄λΈ κ΄κ³μ²λΌ λ€λλ€μμ μΌλλ€, λ€λμΌ κ΄κ³λ‘ νμ΄μΌνλ€.

```java
// λ€λλ€ νμ
@Entity
public class Member{
    @Id @Column(name="MEMBER_ID")
    private String id;
    // μ­λ°©ν₯
    @OneToMany(mappedBy="member")
    private List<MemberProduct> memberProducts;

}
// νμκ³Ό νμμνμ μλ°©ν₯ κ΄κ³λ‘ λ§λ€μλ€. νμμν μν°ν°μͺ½μ΄ μΈλ ν€λ₯Ό κ°μ§κ³  μμΌλ―λ‘ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄λ€. λ°λΌμ μ°κ΄κ΄κ³μ μ£ΌμΈμ΄ μλ νμμ Member.memberProductsμλ mappedByλ₯Ό μ¬μ©νλ€,

//  λ€λλ€ μν
@Entity
public class Product{
    @Id @Column(name="PRODUCT_ID")
    private String id;
    private String name;
}
// μν μν°ν°μμ νμμν μν°ν°λ‘ κ°μ²΄ κ·Έλν νμ κΈ°λ₯μ΄ νμνμ§ μλ€κ³  νλ¨ν΄μ μ°κ΄κ΄κ³λ₯Ό λ§λ€μ§ μμλ€.

// νμμν μν°ν°
@Entity
@IdClass(MemberProductId.class)
public class MemberProduct{

    @Id
    @ManyToOne
    @JoinColumn(name="MEMBER_ID")
    private Member member; // MemberProductId.memberμ μ°κ²°
    @Id
    @ManyToOne
    @JoinColumn(name="PRODUCT_ID")
    private Product product; // MemberProductId.productμ μ°κ²°
    private int orderAmount;
}

// νμμν μλ³μ ν΄λμ€
public class MemberProductId implements Serializable{
    private String member; // MemberProduct.memberμ μ°κ²°
    private String product; // MemberProduct.productμ μ°κ²°

    // hashCode and equals

    @Override
    public boolean equals(Object O){...}

    @Override
    public int hashCode() {...}
}
```
> νμμν(MemberProduct) μν°ν°λ₯Ό λ³΄λ©΄ κΈ°λ³Έ ν€λ₯Ό λ§€ννλ @Idμ μΈλ ν€λ₯Ό λ§€ννλ @JoinColumnμ λμμ μ¬μ©ν΄μ κΈ°λ³Έ ν€ + μΈλ ν€λ₯Ό νλ²μ λ§€ννλ€. κ·Έλ¦¬κ³  @IdClassλ₯Ό μ¬μ©ν΄μ λ³΅ν© κΈ°λ³Έ ν€λ₯Ό λ§€ννλ€.

### π **λ³΅ν© κΈ°λ³Έ ν€**
- νμμν μν°ν°λ κΈ°λ³Έ ν€κ° MEMBER_IDμ PRODUCT_IDλ‘ μ΄λ£¨μ΄μ§ λ³΅ν© κΈ°λ³Έν€(κ°λ¨ν λ³΅ν©ν€)λ€. JPAμμ λ³΅ν© ν€λ₯Ό μ¬μ©νλ €λ©΄ λ³λμ μλ³μ ν΄λμ€λ₯Ό λ§λ€μ΄μΌ νλ€.
- κ·Έλ¦¬κ³  μν°ν°μ @IdClassλ₯Ό μ¬μ©ν΄μ μλ³μ ν΄λμ€λ₯Ό μ§μ νλ©΄ λλ€. μ¬κΈ°μλ MemberProductId ν΄λμ€λ₯Ό λ³΅ν© ν€λ₯Ό μν μλ³μ ν΄λμ€λ‘ μ¬μ©νλ€.
- **λ³΅ν© ν€λ₯Ό μν μλ³μ ν΄λμ€λ λ€μκ³Ό κ°μ νΉμ§μ΄ μλ€.**
    - λ³΅ν© ν€λ λ³λμ μλ³μ ν΄λμ€λ‘ λ§λ€μ΄μΌ νλ€.
    - Serializableμ κ΅¬νν΄μΌ νλ€.
    - equalsμ hashCode λ©μλλ₯Ό κ΅¬νν΄μΌ νλ€.
    - κΈ°λ³Έ μμ±μκ° μμ΄μΌ νλ€.
    - μλ³μ ν΄λμ€λ pulicμ΄μ΄μΌ νλ€.
    - @IdClassλ₯Ό μ¬μ©νλ λ°©λ² μΈμ @EmbeddedIdλ₯Ό μ¬μ©νλ λ°©λ²λ μλ€.
### μλ³ κ΄κ³
- νμμνμ νμκ³Ό μνμ κΈ°λ³Έ ν€λ₯Ό λ°μμ μμ μ κΈ°λ³Έ ν€λ‘ μ¬μ©νλ€.
- μ΄λ κ² λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€λ₯Ό λ°μμ μμ μ κΈ°λ³Έ ν€ + μΈλ ν€λ‘ μ¬μ©νλ κ²μ λ°μ΄ν°λ² μ΄μ€ μ©μ΄λ‘ μλ³ κ΄κ²(Identifying Relationship)λΌ νλ€.
- **νμμν(MemberProduct)μ νμμ κΈ°λ³Έ ν€λ₯Ό λ°μμ μμ μ κΈ°λ³Έ ν€λ‘ μ¬μ©ν¨κ³Ό λμμ νμκ³Όμ κ΄κ³λ₯Ό μν μΈλ ν€λ‘ μ¬μ©νλ€. κ·Έλ¦¬κ³  μνμ κΈ°λ³Έ ν€λ λ°μμ μμ μ κΈ°λ³Έ ν€λ‘ μ¬μ©ν¨κ³Ό λμμ μνκ³Όμ κ΄κ³λ₯Ό μν μΈλ ν€λ‘ μ¬μ©νλ€. λν MemberProductId μλ³μ ν΄λμ€λ‘ λ κΈ°λ³Έ ν€λ₯Ό λ¬Άμ΄μ λ³΅ν© κΈ°λ³Έ ν€λ‘ μ¬μ©νλ€.**

> μ¬μ©λ°©λ²_μ μ₯
```java
public void save(){
    // νμ μ μ₯
    Member member1 = new Member();
    member1.setId("member1");
    member1.setUsername("νμ1");
    em.persist(member1);

    // μν μ μ₯
    Product productA = new Product();
    productA.setId("productA");
    productA.setName("μν1");
    em.persist(productA);

    // νμμν μ μ₯
    MemberProduct memberProduct = new MemberProduct();
    memberProduct.setMember("member1"); // μ£Όλ¬Έ νμ - μ°κ΄κ΄κ³ μ€μ  
    memberProduct.setProduct(productA); // μ£Όλ¬Έ μν - μ°κ΄κ΄κ³ μ€μ 
    memberProduct.setOrderAmount(2); // μ£Όλ¬Έ μλ

    em.persist(memberProduct);
}
```
- νμμν μν°ν°λ₯Ό λ§λ€λ©΄μ μ°κ΄λ νμ μν°ν°μ μν μν°ν°λ₯Ό μ€μ νλ€.
- νμμν μν°ν°λ λ°μ΄ν°λ² μ΄μ€μ μ μ₯λ  λ μ°κ΄λ νμμ μλ³μμ μνμ μλ³μλ₯Ό κ°μ Έμμ μμ μ κΈ°λ³Έ ν€ κ°μΌλ‘ μ¬μ©νλ€.
  
> μ¬μ©λ°©λ²_μ‘°ν
```java
public void find(){
    // κΈ°λ³Έ ν€ κ° μμ±
    MemberProductId memberProductId = new MemberProduct();
    memberProductId.setMember("member1");
    memberProductId.setProduct(productA); 

    MemberProduct memberProduct = em.find(MemberProduct.class, memberProductId);

    Member member = memberProduct.getMember();
    Product product = memberProduct.getProduct();

}
```
- λ³΅ν© ν€λ ν­μ μλ³μ ν΄λμ€λ₯Ό λ§λ€μ΄μΌ νλ€. em.find()λ₯Ό λ³΄λ©΄ μμ±ν μλ³μ ν΄λμ€λ‘ μν°ν°λ₯Ό μ‘°ννλ€.
- λ³΅ν© ν€λ₯Ό μ¬μ©νλ λ°©λ²μ λ³΅μ‘νλ€. λ¨μν μ»¬λΌ νλλ§ κΈ°λ³Έ ν€λ‘ μ¬μ©νλ κ²κ³Ό λΉκ΅ν΄μ λ³΅ν© ν€λ₯Ό μ¬μ©νλ©΄ ORM λ§€νμμ μ²λ¦¬ν  μΌμ΄ μλΉν λ§μμ§λ€.
- λ³΅ν© ν€λ₯Ό μν μλ³μ ν΄λμ€ μμ±κ³Ό @IdClass λλ @EmbeddedIdλ μ¬μ©ν΄μΌ νλ€. κ·Έλ¦¬κ³  μλ³μ ν΄λμ€μ equals, hashCodeλ κ΅¬νν΄μΌ νλ€.

### π₯ λ€λλ€: μλ‘μ΄ κΈ°λ³Έ ν€ μ¬μ©
  
![image](https://user-images.githubusercontent.com/71022555/174605064-35df1d9b-5d07-4a96-9ebf-d58f3dc4ee2e.png)  
  
- λ°μ΄ν°λ² μ΄μ€μμ μλμΌλ‘ μμ±ν΄μ£Όλ λλ¦¬ ν€λ₯Ό Long κ°μΌλ‘ μ¬μ©νλ κ²μ μΆμ²νλ€.
    - κ°νΈνκ³  κ±°μ μκ΅¬ν μΈ μ μμΌλ©° λΉμ¦λμ€μ μμ‘΄νμ§ μλλ€.
    - ORM λ§€ν μμ λ³΅ν© ν€λ₯Ό λ§λ€μ§ μμλ λλ―λ‘ κ°λ¨ν λ§€νμ μμ±ν  μ μλ€.
- μ κ·Έλ¦Όμ ORDER_IDλΌλ μλ‘μ΄ κΈ°λ³Έ ν€λ₯Ό νλ λ§λ€κ³  MEMBER_ID, PRODUCT_ID μ»¬λΌμ μΈλ ν€λ‘λ§ μ¬μ©νλ€.

```java
// μ£Όλ¬Έ(ORDER νμ΄λΈ) μ½λ
@Entity
public class Order{
    @Id @GeneratedValue
    @Column(name="ORDER_ID")
    private Long id;

    @ManyToOne
    @JoinColumn(name="MEMBER_ID")
    private Member member;

    @ManyToOne
    @JoinColumn(name="PRODUCT_ID")
    private Product product;

    private int orderAmount;

}
```
- λλ¦¬ ν€λ₯Ό μ¬μ©ν¨μΌλ‘μ¨ μ΄μ μ λ³΄μλ μλ³ κ΄κ³μ λ³΅ν© ν€λ₯Ό μ¬μ©νλ κ²λ³΄λ€ λ§€νμ΄ λ¨μνκ³  μ΄ν΄νκΈ° μ½λ€.
- νμ μν°ν°μ μν μν°ν°λ λ³κ²½μ¬ν­μ΄ μλ€.
```java
// νμ
@Entity
public class Member{
    @Id @Column(name="MEMBER_ID")
    private String id;
    // μ­λ°©ν₯
    @OneToMany(mappedBy="member")
    private List<MemberProduct> memberProducts;

}

//  μν
@Entity
public class Product{
    @Id @Column(name="PRODUCT_ID")
    private String id;
    private String name;
}
```
μ¬μ©λ°©λ²_μ μ₯
```java
public void save(){
    // νμ μ μ₯
    Member member1 = new Member();
    member1.setId("member1");
    member1.setUsername("νμ1");
    em.persist(member1);

    // μν μ μ₯
    Product productA = new Product();
    productA.setId("productA");
    productA.setName("μν1");
    em.persist(productA);

    // μ£Όλ¬Έ μ μ₯
    Order order = new Order();
    order.setMember(member1); // μ£Όλ¬Έ νμ - μ°κ΄κ΄κ³ μ€μ 
    order.setProduct(productA); // μ£Όλ¬Έ μν - μ°κ΄κ΄κ³ μ€μ 
    order.setOrderAmount(2); // μ£Όλ¬Έ μλ
    em.persist(order);
}
```
μ¬μ©λ°©λ²_μ‘°ν
```java
public void find(){
    Long orderId = 1L;
    Order order = em.find(Order.class, orderId);

    Member member = order.getMember();
    Product product = order.getProduct();
}

```

### π? λ€λλ€ μ°κ΄κ΄κ³ μ λ¦¬
> λ€λλ€ κ΄κ³λ₯Ό μΌλλ€ λ€λμΌ κ΄κ³λ‘ νμ΄λ΄κΈ° μν΄ μ°κ²° νμ΄λΈμ λ§λ€ λ μλ³μλ₯Ό μ΄λ»κ² κ΅¬μ±ν μ§ μ νν΄μΌ νλ€.
1. μλ³ κ΄κ³ : λ°μμ¨ μλ³μλ₯Ό κΈ°λ³Έ ν€ + μΈλ ν€λ‘ μ¬μ©νλ€.
2. λΉμλ³ κ΄κ³ : λ°μμ¨ μλ³μλ μΈλ ν€λ‘λ§ μ¬μ©νκ³  μλ‘μ΄ μλ³μλ₯Ό μΆκ°νλ€.
- DB μ€κ³μμλ 1λ²μ²λΌ λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€λ₯Ό λ°μμ μμ νμ΄λΈμ κΈ°λ³Έ ν€ + μΈλ ν€λ‘ μ¬μ©νλ κ²μ μλ³ κ΄κ³λΌ νκ³  2λ²μ²λΌ λ¨μν μΈλν€λ‘λ§ μ¬μ©νλ κ²μ λΉμλ³ κ΄κ³λΌ νλ€.
- κ°μ²΄ μμ₯μμ λ³΄λ©΄ 2λ²μ²λΌ λΉμλ³ κ΄κ³λ₯Ό μ¬μ©νλ κ²μ΄ λ³΅ν© ν€λ₯Ό μν μλ³μ ν΄λμ€λ₯Ό λ§λ€μ§ μμλ λλ―λ‘ λ¨μνκ³  νΈλ¦¬νκ² ORM λ§€νμ ν  μ μλ€. μ΄λ° μ΄μ λ‘ 2λ² λΉμλ³ κ΄κ³λ₯Ό μΆμ²ν¨

### μ λ¦¬
> μ΄λ²μλ λ€λμΌ, μΌλλ€, μΌλμΌ, λ€λλ€ μ°κ΄κ΄κ³λ₯Ό λ¨λ°©ν₯, μλ°©ν₯μΌλ‘ λ§€ννλ λ°©λ²μ λν΄ μμλ³΄μλ€. λ§μ§λ§μλ λ€λλ€ μ°κ΄κ΄κ³λ₯Ό μΌλλ€, λ€λμΌ μ°κ΄κ΄κ³λ‘ νμ΄λ³΄μλ€.

μ°Έκ³ μλ£
---
##### μλ° ORM νμ€ JPA νλ‘κ·Έλλ°
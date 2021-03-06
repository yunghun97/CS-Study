# ๐ฎ JPA N+1 ๋ฌธ์ 

### ์ฐ๊ด ๊ด๊ณ์์ ๋ฐ์ํ๋ ์ด์๋ก ์ํฐํฐ๋ฅผ ์กฐํํ  ๊ฒฝ์ฐ์ ์กฐํ๋ ๋ฐ์ดํฐ ๊ฐฏ์(n) ๋งํผ ์ฐ๊ด๊ด๊ณ์ ์กฐํ ์ฟผ๋ฆฌ๊ฐ ์ถ๊ฐ๋ก ๋ฐ์ํ์ฌ ๋ฐ์ดํฐ๋ฅผ ์ฝ์ด์ค๊ฒ ๋๋ค. ์ด๋ฅผ N+1 ๋ฌธ์ ๋ผ๊ณ  ํ๋ค.

## ์ฐ๊ด๊ด๊ณ  
![image](https://user-images.githubusercontent.com/71022555/172904624-1be430ec-28c5-4d8b-a5e5-3bb129e77a9c.png)  
  
```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 10, nullable = false)
    private String name;

    @OneToMany(mappedBy = "user")
    private Set<Article> articles = emptySet();
}

@Entity
public class Article {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 50, nullable = false)
    private String title;

    @Lob
    private String content;

    @ManyToOne
    private User user;
}
// FetchType ๊ธฐ๋ณธ ๊ฐ : ~ToMany(fetch = FetchType.EAGER)
// FetchType ๊ธฐ๋ณธ ๊ฐ : ~ToOne(fetch = FetchType.LAZY)
```

## ๐ ์ฆ์๋ก๋ฉ(EAGER)  
```java
userRepository.findById("Gwon");
```
- ๋ด๋ถ์ ์ผ๋ก inner join๋ฌธ ํ๋๊ฐ ๋ ์๊ฐ์ User๊ฐ ์กฐํ๋จ๊ณผ ๋์์ Article๊น์ง ์ฆ์๋ก๋ฉ๋๋ ๊ฒ์ ํ์ธํ  ์ ์์
- ๋ฌธ์ ๋ jpql์์ ๋ฐ์ํ๋ค.
    - select u from User u; ๊ตฌ๋ฌธ์์ FetchType๋ EAGER๋ก ์ค์ ๋์ด ์์ผ๋ฏ๋ก selectํ ๋ชจ๋  User์ ๋ํด์ article๊ฐ ์๋์ง ๊ฒ์์ ์งํํ๊ฒ ๋๋ค.
- ๋ฐ๋ผ์ ์์์ ๋ญ๋น๊ฐ ๋ฐ์ํ๋ค.

## ๐ ์ง์ฐ๋ก๋ฉ(LAZY)
```java
List<User> users = userRepository.findAll();
for(User user : users){ // N+1 ๋ฌธ์  ๋ฐ์ 
    System.out.println(user.articles().size());
}
```
- ์ฐ๊ด๋ ๊ฐ์ฒด๋ฅผ "์ฌ์ฉ"ํ๋ ์์ ์ ๋ก๋ฉ์ ํด์ฃผ๋ ๋ฐฉ๋ฒ
- ์ง์ฐ๋ก๋ฉ์ ์ค์ ํ๋๋ผ๋ N+1์ ๋ฐ์ํ  ์ ์๋ค!
    - ํด๋น ์ฐ๊ฒฐ entity์ ๋ํด ํ๋ก์๋ฅผ ๊ฑธ์ด๋๊ณ , ์ฌ์ฉํ  ๋ ์ฟผ๋ฆฌ๋ฌธ์ ๊ฒฐ๊ตญ ๋ ๋ฆฌ๊ธฐ ๋๋ฌธ์ ์ฒ์ findํ  ๋๋ N+1์ด ๋ฐ์ํ์ง ์์ง๋ง ์ถ๊ฐ๋ก user ๊ฒ์ ํ User์ Article์ ์ฌ์ฉํด์ผ ํ๋ค๋ฉด ์ด๋ฏธ ์บ์ฑ๋ User์ Article ํ๋ก์์ ๋ํ ์ฟผ๋ฆฌ๊ฐ ๋ ๋ฐ์ํ๊ฒ ๋ฉ๋๋ค.
- ์ง์ฐ๋ก๋ฉ์ ํ์ผ๋ ์ง์ฐ๋ก๋ฉ ๋์์ธ Article์ ๋์ค์ ์กฐํํด์ ์ฟผ๋ฆฌ๊ฐ ๋ ๋ ์๊ฐ๋ ๊ฒ์กฐ์ฐจ๋ N+1 ๋ฌธ์ ์ ์ ์ค ํ๋

## โจ ์ง์ฐ๋ก๋ฉ์์์ ํด๊ฒฐ์ฑ - fetch join
### N+1 ์ด ๋ฐ์ํ๋ ์์ธ
> JPA๊ฐ ์๋์ผ๋ก ๋จผ์  ์์ฑํด์ฃผ๋ Jpql์ ํตํด์ ์ฐ์ ์ ์ผ๋ก ์ฟผ๋ฆฌ๋ฅผ ๋ง๋ค๋ค๋ณด๋ ์ฐ๊ด๊ด๊ณ๊ฐ ๊ฑธ๋ ค์์ด๋ join์ด ๋ฐ๋ก ๊ฑธ๋ฆฌ์ง ์๋๋ค.
```java
// N+1 ๋ฌธ์  ๋ฐ์ ์ฝ๋ (ํ๋ก์๋ก ๊ฐ์ ธ์ค๋๊ฑด ๋ณํจ์ด ์์)
@Query("select distinct u from User u left join u.articles")
List<User> findAllJPQL();
```

## fecth join ๋ฐฉ๋ฒ 1
```java
@Query("select distinct u from User u left join fetch u.articles")
List<User> findAllJPQLFetch();
```

```java
List<User> users = userRepository.findAll();
for(User user : users){ // N+1 ๋ฌธ์  ๋ฐ์ 
    System.out.println(user.articles().size());
}
// ์ฟผ๋ฆฌ๋ฅผ ๋ ๋ฆด ๋ article์ ํ๋ฒ์ ๊ฐ์ ธ์จ๋ค.
```
  
## fetch join ๋ฐฉ๋ฒ 2 **@EntityGraph**
- ๊ธฐ์กด์ Jpql fetch join ์ฌ์ฉ ์ ํ๋์ฝ๋ฉ์ ํ๊ฒ ๋๋ค๋ ๋จ์ ์ด ์์ต๋๋ค.
```java
@EntityGraph(attributePaths = {"articles"}, type = EntityGraphType.FETCH)
@Query("select distinct u from User u left join u.articles")
List<User> findAllEntityGraph();
// Jpql ๊ตฌ๋ฌธ์์์ fetch๋ ์กด์ฌํ์ง ์์ง๋ง ๊ธฐ์กด๊ณผ ๋ง์ฐฌ๊ฐ์ง๋ก fetch join์ ํตํด ๋ฐ๋ก ์กฐํํ  ์ ์์
```

## โ fetch join ์ฃผ์ํ  ์ 
## ๐ ๋ฌธ์ ์  1. Pagination
- Page๋ฅผ ๋ฐํํ๋ ์ฟผ๋ฆฌ๋ฅผ ์์ฑ ์ ํด๋น ์ค๋ฅ๊ฐ ๋ฐ์
```java
@EntityGraph(attributePaths = {"articles"}, type = EntityGraphType.FETCH)
@Query("select distinct u from User u left join u.articles")
Page<User> findAllPage(Pageable pageable);
```
```java
void pagingFetchJoinTest() {
    PageRequest pageRequest = PageRequest.of(0, 2);
    Page<User> users = userRepository.findAllPage(pageRequest);
    for (User user : users) {
        System.out.println(user.articles().size());
    }
}
```
- ํด๋น ์ฟผ๋ฆฌ๋ฅผ ์์ธํ ๋ณด๋ฉด ํ์ด์ง ์ฒ๋ฆฌ๋ฅผ ํ  ๋ ์ฌ์ฉํ๋ Limit, Offset์ด ์์ต๋๋ค. 
- user.articleSize()์ ์ ์์ ์ผ๋ก ์๋
```bash
WARN 79170 --- [    Test worker] o.h.h.internal.ast.QueryTranslatorImpl   : HHH000104: firstResult/maxResults specified with collection fetch; applying in memory!
# applying in memory, ์ฆ ์ธ๋ฉ๋ชจ๋ฆฌ๋ฅผ ์ ์ฉํด์ ์กฐ์ธ์ ํ๋ค๊ณ  ํฉ๋๋ค.
```
> **์ฆ List์ ๋ชจ๋  ๊ฐ์ selectํด์ ์ธ๋ฉ๋ชจ๋ฆฌ์ ์ ์ฅํ๊ณ  application ๋จ์์ ํ์ํ ํ์ด์ง๋งํผ ๋ฐํ์ ์งํํด์ค -> Paging์ ํ ์ด์ ๊ฐ ์์ด์ง + Out Of Memory ๋ฐ์ ํ๋ฅ  ์์น**
  
> ๋ฐ๋ผ์ Pagination์์๋ fetch join์ ํ๊ณ ์ถ์ด์ ํ๋ค๊ณ  ํ๋๋ผ๊ณ  ํด๊ฒฐ์ ํ  ์ ์์ต๋๋ค.

```
์งง๊ฒ ์ด์ผ๊ธฐ๋ฅผ ํ์๋ฉด fetch join์์ distinct๋ฅผ ์ฐ๋ ๊ฒ๊ณผ ์ฐ๊ด์ด ์์ต๋๋ค. distinct๋ฅผ ์ฐ๋ ์ด์ ๋ ํ๋์ ์ฐ๊ด๊ด๊ณ์ ๋ํด์ fetch join์ผ๋ก ๊ฐ์ ธ์จ๋ค๊ณ  ํ์ ๋ ์ค๋ณต๋ ๋ฐ์ดํฐ๊ฐ ๋ง๊ธฐ ๋๋ฌธ์ ์ค์ ๋ก ์ํ๋ ๋ฐ์ดํฐ์ ์๋ณด๋ค ์ค๋ณต๋์ด ๋ง์ด ๋ค์ด์ค๊ฒ ๋ฉ๋๋ค.

๊ทธ ์ด์ ๋๋ฌธ์ ๊ฐ๋ฐ์๊ฐ ์ง์  distinct๋ฅผ ํตํด์ jpa์๊ฒ ์ค๋ณต ์ฒ๋ฆฌ๋ฅผ ์ง์ํ๊ฒ ๋๋ ๊ฒ์ด๊ณ , Paging์ฒ๋ฆฌ๋ ์ฟผ๋ฆฌ๋ฅผ ๋ ๋ฆด ๋ ์งํ๋๊ธฐ ๋๋ฌธ์ jpa์๊ฒ pagination ์์ฒญ์ ํ์ฌ๋ jpa๋ distinct๋์ ๋ง์ฐฌ๊ฐ์ง๋ก ์ค๋ณต๋ ๋ฐ์ดํฐ๊ฐ ์์ ์ ์์ผ๋ limit offset์ ๊ฑธ์ง ์๊ณ  ์ผ๋จ ์ธ๋ฉ๋ชจ๋ฆฌ์ ๋ค ๊ฐ์ ธ์์ application์์ ์ฒ๋ฆฌํ๋ ๊ฒ์ด์ฃ .
```
  
## ๐ Pagination ํด๊ฒฐ์ฑ 1 : ~ToOne ๊ด๊ณ์์ ํ์ด์ง ์ฒ๋ฆฌ
- User์์ ๊ฒ์ํ์ง ์๊ณ  Atricle์ User์ ๋ํด์ @ManyToOne ์ฐ๊ด๊ด๊ณ์ด๊ธฐ ๋๋ฌธ์ Pagination์ ์งํํด๋ ์ธ๋ฉ๋ชจ๋ฆฌ์์ ๋ชจ๋  Article์ ์กฐํํ๋ ๊ฒ์ด ์๋ limit์ ๊ฑธ์ด ํ์ํ ๋ฐ์ดํฐ๋ง ๊ฐ์ ธ์ฌ ์ ์์ต๋๋ค.
## ๐ Pagination ํด๊ฒฐ์ฑ 2 : Batch Size
- ์ปฌ๋์ ์กฐ์ธ์ ํ๋ ๊ฒฝ์ฐ์๋ **fetch join์ ์์ ์ฌ์ฉํ์ง ์๊ณ  ์กฐํํ  ์ปฌ๋์ ํ๋์ ๋ํด์ @BatchSize** ๋ฅผ ๊ฑธ์ด ํด๊ฒฐํฉ๋๋ค.
```java
@BatchSize(size=100)
@OneToMany(mappedBy="user", fetch=FetchType.LAZY)
private Set<Article> articles = emptySet();

Page(User) findAll(Pageable pageable);
```
- article์ selectํ๋ ์ฟผ๋ฆฌ๊ฐ ํ๋ ๋ฑ์ฅํฉ๋๋ค.
- ์ง์ฐ๋ก๋ฉํ๋ ๊ฐ์ฒด์ ๋ํด์ Batch์ฑ loading์ ํ๋ ๊ฒ์ด๋ผ๊ณ  ์๊ฐํ๋ฉด ๋ฉ๋๋ค.,
- ๊ธฐ์กด์ ์ง์ฐ๋ก๋ฉ์ ๋ํด์๋ ๊ฐ์ฒด๋ฅผ ์กฐํํ  ๋ ๊ทธ๋๊ทธ๋ ์ฟผ๋ฆฌ๋ฌธ์ ๋ ๋ ค์ N+1 ๋ฌธ์ ๊ฐ ๋ฐ์ํ ๋ฐ๋ฉด 
- ๊ฐ์ฒด๋ฅผ ์กฐํํ๋ ์์ ์ ์ฟผ๋ฆฌ๋ฅผ ํ๋๋ง ๋ ๋ฆฌ๋๊ฒ ์๋๋ผ ํด๋นํ๋ Article์ ๋ํด์ ์ฟผ๋ฆฌ๋ฅผ batch size๊ฐ๋ฅผ ๋ ๋ฆฌ๋ ๊ฒ์๋๋ค.
```sql
-- batch ์ฟผ๋ฆฌ์์ where๋ถ๋ถ
where
	articles0_.user_id in (
		?, ?
	)
```
- in(?, ?)๊ฐ ๊ฒฐ๊ตญ user id๋ฅผ 100๊ฐ๋ฅผ ๊ฐ์ ธ์ค๋ ์ฟผ๋ฆฌ๋ฌธ์ผ๋ก์จ ๊ทธ๋๊ทธ๋ ์กฐํํ๋ ๊ฒ์ด ์๋ ์กฐํํ  ๋ ๊ทธ๋ฅ **batch size**๋งํผ ํ๋ฒ์ ๊ฐ์ ธ์์ ๋ค์ ์๊ธธ ์ง์ฐ๋ก๋ฉ์ ๋ํด์ ๋ฏธ์ฐ์ ๋ฐฉ์ง(batch size๋ Article์ด ์๋  User ๊ธฐ์ค)
> ์ต์ ํ๋ ๋ฐ์ดํฐ ์ฌ์ด์ฆ๋ฅผ ์์ง ๋ชปํ๋ค๋ฉด ์ ์ ํ์ง ์์ ์ ์๋ค.
  
## ๐ Pagination ํด๊ฒฐ์ฑ 3 : @Fetch(FetchMode.SUBSELECT)
- @BatchSize์ ๋น์ทํ์ง๋ง ๋ค๋ฅธ ์ด๋ธํ์ด์์๋๋ค.
- @BatchSize์ ๊ฒฝ์ฐ ์ฌ์ด์ฆ ๊ฐฏ์ ์ ํ์ ์์๋ก ๋์ด์ ์ฌ์ฉ์๊ฐ ์ต์ ํ๋ ๋ฐ์ดํฐ ์ฌ์ด์ฆ๋ฅผ ์ ์ฉํ๊ฒ๋ ๋์์ค๋ค๋ฉด ์ด ์ด๋ธํ์ด์์ ๊ทธ๋ฅ ์ ๋ถ ์งํ
```java
@Fetch(FetchMode.SUBSELECT)
@OneToMany(mappedBy="user", fetch=FetchType.LAZY)
private Set<Article> articles = emptySet();
```
- @BathSize(size="INF") ๋๋
- Batch Size์ ๊ฒฝ์ฐ ์ฃผ์ด์ง size๋งํผ User ID๋ฅผ ์๋ ฅํ์ฌ ํ๋ก์ ์ํ์ ๋ฐ๋ผ ์ง์ฐ๋ก๋ฉ์ ์งํํ๋ค๋ฉด ์ง๊ธ์ User Id๋ฅผ ์ ๋ถ ์กฐํํ๋ค.

## ๐ ๋ฌธ์ ์  2. ๋ ์ด์์ Collection fetch join(~ToMany) ๋ถ๊ฐ๋ฅ

- fetch join์ ํ  ๋ ToMany์ ๊ฒฝ์ฐ ํ๋ฒ์ fetch join์ ๊ฐ์ ธ์ค๊ธฐ ๋๋ฌธ์ collection join์ด 2๊ฐ์ด์์ด ๋  ๊ฒฝ์ฐ ๋๋ฌด ๋ง์ ๊ฐ์ด ๋ฉ๋ชจ๋ฆฌ๋ก ๋ค์ด์ exception(MultipleBagFetchException)์ด ์ถ๊ฐ๋ก ๊ฑธ๋ฆฝ๋๋ค.
- 2๊ฐ ์ด์์ bags, ์ฆ collection join์ด ๋๊ฐ์ด์์ผ ๋ exception์ด ๋ฐ์ํฉ๋๋ค.
์ฐธ๊ณ ์๋ฃ
- ~ToOne์ ์ผ๋ง๋งํผ fetch join์ ํด๋ ๊ด์ฐฎ์ง๋ง ~ToMany๋ ํ๋์ผ ๋๋ ์ธ๋ฉ๋ชจ๋ฆฌ์์ ์ฒ๋ฆฌํ๊ณ  ๋ ๊ฐ์ด์์ Exception์ผ๋ก ์ ํ
```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 10, nullable = false)
    private String name;
    
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Article> articles = new ArrayList<>();
    
    @OneToMany(mappedBy = "question", fetch = FetchType.LAZY)
    private List<Question> questions = new ArrayList<>();
}

@EntityGraph(attributePaths = {"articles", "questions"}, type = EntityGraphType.FETCH)
@Query("select distinct u from User u left join u.articles")
List<User> findAllEntityGraph2();

// org.hibernate.loader.MultipleBagFetchException: cannot simultaneously fetch multiple bags: [com.example.jpa.domain.User.articles, com.example.jpa.domain.User.questions]; nested exception is java.lang.IllegalArgumentException: org.hibernate.loader.MultipleBagFetchException: cannot simultaneously fetch multiple bags: [com.example.jpa.domain.User.articles, com.example.jpa.domain.User.questions] ์๋ฌ ๋ฐ์
```
### Collection fetch join ํด๊ฒฐ์ฑ 1. ์๋ฃํ์ Set
- Set์ ์ฌ์ฉํ๊ฒ ๋๋ค๋ฉด HashSet์ผ๋ก๋ ์์๊ฐ ์ค์ํ ๋ฐ์ดํฐ์๋ ์์๋ฅผ ๋ณด์ฅํ  ์ ์๊ธฐ ๋๋ฌธ์ LinkedHashSet์ ์ฌ์ฉ๊ถ์ฅ
``` 
๋ค๋ง Set์ ์ฌ์ฉํ๋ค๊ณ  ํด์, Pagination์ ๋ง์ฐฌ๊ฐ์ง๋ก ํด๊ฒฐ์ด ๋ถ๊ฐ๋ฅํฉ๋๋ค.

Pagination์ ๊ทผ๋ณธ์ ์ผ๋ก ๋ช๊ฐ์ collection join ์๋ ๊ฐ์ ์ธ๋ฉ๋ชจ๋ฆฌ์์ ๊ฐ์ ธ์ค๊ธฐ ๋๋ฌธ์ OOM์ ๋ฐ์์ํฌ ์ ์๋ ์์ธ์ด ๋์ด ํด๋น ๋ฐฉ๋ฒ์ผ๋ก๋ ํด๊ฒฐ์ด ๋ถ๊ฐ๋ฅํฉ๋๋ค.

์ค๋ น Collection join์ด ํ ๊ฐ์ธ ์ํฉ์์ Set ์๋ฃ๊ตฌ์กฐ๋ฅผ ์ฌ์ฉํ๋ค๊ณ  ํด๋ ์ธ๋ฉ๋ชจ๋ฆฌ์์ ๊ฐ์ ธ์ต๋๋ค.

HHH000104: firstResult/maxResults specified with collection fetch; applying in memory!
```
### Collection fetch join ํด๊ฒฐ์ฑ 1. BatchSize
- Pagination์ ํด๊ฒฐ์ฑ ์ด์ง๋ง Collection join์ด ๋ ๊ฐ ์ด์์ผ ๋ MultipleBagFetchException์ ํด๊ฒฐํ  ์ ์๋ ๋ฐฉ๋ฒ์ด๊ธฐ๋ ํฉ๋๋ค.
```java
@BatchSize(size = 100)
@OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
private Set<Article> articles = emptySet();

@Query("select distinct u from User u left join u.articles left join u.questions")
Page<User> findAllPage2(Pageable pageable);
```
> **์ฃผ์ํด์ผํ  ์ ์ batch size์ fetch join์ ๊ฑธ๋ฉด ์๋ฉ๋๋ค.**
```
fetch join์ด ์ฐ์ ์๋์ด ์ ์ฉ๋๊ธฐ ๋๋ฌธ์ batch size๊ฐ ๋ฌด์๋๊ณ  fetch join์ ์ธ๋ฉ๋ชจ๋ฆฌ์์ ๋จผ์  ์งํํ์ฌ List๊ฐ MultipleBagFetchException๊ฐ ๋ฐ์ํ๊ฑฐ๋, Set์ ์ฌ์ฉํ ๊ฒฝ์ฐ์๋ Pagination์ ์ธ๋ฉ๋ชจ๋ฆฌ ๋ก๋ฉ์ ์งํํฉ๋๋ค.
```

## ๐ ์ขํฉ
- **์ฆ์๋ก๋ฉ**
    - jpql์ ์ฐ์ ์ ์ผ๋ก selectํ๊ธฐ ๋๋ฌธ์ ์ฆ์๋ก๋ฉ์ ์ดํ์ ๋ณด๊ณ  ๋๋ค๋ฅธ ์ฟผ๋ฆฌ๊ฐ ๋ ์๊ฐ N+1
- **์ง์ฐ๋ก๋ฉ**
    - ์ง์ฐ๋ก๋ฉ๋ ๊ฐ์ selectํ  ๋ ๋ฐ๋ก ์ฟผ๋ฆฌ๊ฐ ๋ ์๊ฐ N+1
- **fetch join**
    - ์ง์ฐ๋ก๋ฉ์ ํด๊ฒฐ์ฑ
    - ์ฌ์ฉ๋  ๋ ํ์ ๋ ๊ฐ์ ํ๋ฒ์ join์์ selectํด์ ๊ฐ์ ธ์ด
    - Pagination์ด๋ 2๊ฐ ์ด์์ collection join์์ ๋ฌธ์ ๊ฐ ๋ฐ์
- **Pagination**
    - fetch join ์ limit, offset์ ํตํ ์ฟผ๋ฆฌ๊ฐ ์๋ ์ธ๋ฉ๋ชจ๋ฆฌ์ ๋ชจ๋ ๊ฐ์ ธ์ application๋จ์์ ์ฒ๋ฆฌํ์ฌ OOM ๋ฐ์
    - BatchSize๋ฅผ ํตํด ํ์ ์ ๋ฐฐ์น์ฟผ๋ฆฌ๋ก ์ํ๋ ๋งํผ ์ฟผ๋ฆฌ๋ฅผ ๋ ๋ฆผ > ์ฟผ๋ฆฌ๋ ๋ ์๊ฐ์ง๋ง N๋ฒ ๋งํผ์ ๋ฌด์ํ ์ฟผ๋ฆฌ๋ ๋ฐ์๋์ง ์์
- **2๊ฐ ์ด์์ Collection join**
    - List ์๋ฃ๊ตฌ์กฐ์ 2๊ฐ ์ด์์ Collection join(~ToMany๊ด๊ณ)์์ fetch join ํ  ๊ฒฝ์ฐ MultipleBagFetchException ์์ธ ๋ฐ์
    - Set์๋ฃ๊ตฌ์กฐ๋ฅผ ์ฌ์ฉํ๋ค๋ฉด ํด๊ฒฐ๊ฐ๋ฅ (Pagination์ ์ฌ์ ํ ๋ฐ์)
    - BatchSize๋ฅผ ์ฌ์ฉํ๋ค๋ฉด ํด๊ฒฐ๊ฐ๋ฅ (Pagination ํด๊ฒฐ)



์ฐธ๊ณ ์๋ฃ
---
#### https://incheol-jung.gitbook.io/docs/q-and-a/spring/n+1
#### https://velog.io/@jinyoungchoi95/JPA-%EB%AA%A8%EB%93%A0-N1-%EB%B0%9C%EC%83%9D-%EC%BC%80%EC%9D%B4%EC%8A%A4%EA%B3%BC-%ED%95%B4%EA%B2%B0%EC%B1%85
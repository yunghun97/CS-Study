# ๐ JPA Auditing
  
### Java์์ ORM ๊ธฐ์ ์ธ JPA๋ฅผ ์ฌ์ฉํ์ฌ ๋๋ฉ์ธ์ ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค ํ์ด๋ธ์ ๋งคํํ  ๋ ๊ณตํต์ ์ผ๋ก ๋๋ฉ์ธ๋ค์ด ๊ฐ์ง๊ณ  ์๋ ํ๋๋ ์ปฌ๋ผ๋ค์ด ์กด์ฌํฉ๋๋ค. ex) ์์ฑ, ์์ ์ผ์, ์๋ณ์
### ์ด๋ฅผ JPA์์ Audit(๊ฐ์ํ๋ค, ๊ฐ์ฌํ๋ค)๋ผ๋ ๋ป์ผ๋ก Spring Data JPA์์ ์๊ฐ์ ๋ํด์ ์๋์ผ๋ก ๊ฐ์ ๋ฃ์ด์ฃผ๋ ๊ธฐ๋ฅ์๋๋ค.  
### ๋๋ฉ์ธ์ ์์์ฑ ์ปจํ์คํธ์ ์ ์ฅํ๊ฑฐ๋ ์กฐํ๋ฅผ ์ํํ ํ์ update๋ฅผ ํ๋ ๊ฒฝ์ฐ ๋งค๋ฒ ์๊ฐ ๋ฐ์ดํฐ๋ฅผ ์๋ ฅํด์ฃผ์ด์ผ ํ๋๋ฐ ์ด๋ฅผ audit์ ์ด์ฉํ์ฌ ์๋์ผ๋ก ์๊ฐ์ ๋งคํํ์ฌ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ํ์ด๋ธ์ ๋ฃ์ด์ฃผ๊ฒ ๋ฉ๋๋ค.

> Java 1.8 ์ด์๋ถํฐ๋ LocalDate, LocalDatTime์ ์ฌ์ฉํฉ๋๋ค. ๋ํ LocalDateTime ๊ฐ์ฒด ํ์ด๋ธ ๋งคํ ์ด์๋ Hibernate 5.2๋ถํฐ ํด๊ฒฐ๋์์ต๋๋ค.  

  
## ๐ ์ฌ์ฉ๋ฒ
- JPA ์์กด์ฑ ์ถ๊ฐ
1. ์ด๋ธํ์ด์ ์ถ๊ฐ(@EnableJpaAuditing)
```java
@EnableJpaAuditing
public class Application{
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```  
2. ๊ณตํต์ผ๋ก ์ฌ์ฉํ  ์ํฐํฐ ์ถ๊ฐ
```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseDateEntity {
    @CreatedDate
    private LocalDate createdDate;
    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```
## ๐ถ ์์ฑ
|์์ฑ|์ค๋ช|
|:---|:---|
|@MappedSuperclass|JPA Entity ํด๋์ค๋ค์ด ํด๋น ์ถ์ ํด๋์ค๋ฅผ ์์ํ  ๊ฒฝ์ฐ createDate, modifiedDate๋ฅผ ์ปฌ๋ผ์ผ๋ก ์ธ์|
|@EntityListeners(AuditingEntityListener.class|ํด๋น ํด๋์ค์ Auditing ๊ธฐ๋ฅ์ ํฌํจ|
|@CreatedDate|Entity๊ฐ ์์ฑ๋์ด ์ ์ฅ๋  ๋ ์๊ฐ์ด ์๋ ์ ์ฅ|
|@LastModifiedDate|์กฐํํ Entity์ ๊ฐ์ ๋ณ๊ฒฝํ  ๋ ์๊ฐ์ด ์๋ ์ ์ฅ|

์ฐธ๊ณ ์๋ฃ 
---
#### https://webcoding-start.tistory.com/53
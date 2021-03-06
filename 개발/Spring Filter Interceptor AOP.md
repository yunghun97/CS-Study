# ๐ Spring Filter Interceptor AOP

### ์๋ฐ ์น ๊ฐ๋ฐ์ ํ๋ค๋ณด๋ฉด, **๊ณตํต์ **์ผ๋ก ์ฒ๋ฆฌํด์ผ ํ  ์๋ฌด๋ค์ด ๋ง๋ค.

> ์ฆ ๊ณตํต ๋ถ๋ถ๋ค์ ๋นผ์ ๋ฐ๋ก ๊ด๋ฆฌํ๋ ๊ฒ์ด ํจ์จ์ ์ด๋ค.
  
### ๊ณตํต ์ฒ๋ฆฌ ๋ฐฉ๋ฒ 
1. Filter
2. Interceptor
3. AOP  
  

## ํ๋ฆ
![image](https://user-images.githubusercontent.com/71022555/172141138-28323e4d-5f95-480b-9ad3-3fc28decefbd.png)  
  
- Interceptor์ Filter๋ Servlet ๋จ์์์ ์คํ๋๋ค.
- AOP๋ ๋ฉ์๋ ์์ Proxy ํจํด์ ํํ๋ก ์คํ๋๋ค.
- ์คํ ์์ Filter -> Interceptor -> AOP -> Interceptor -> Filter
  
1. ์๋ฒ๋ฅผ ์คํ์์ผ ์๋ธ๋ฆฟ์ด ์ฌ๋ผ์ค๋ ๋์์ init์ด ์คํ๋๊ณ  ๊ทธ ํ doFilter๊ฐ ์คํ๋๋ค.
2. ์ปจํธ๋กค๋ฌ์ ๋ค์ด๊ฐ๊ธฐ ์  preHandler๊ฐ ์คํ๋๋ค.
3. ์ปจํธ๋กค๋ฌ์์ ๋์ postHandler, after Completion, doFilter ์์ผ๋ก ์งํ์ด๋๋ค.
4. ์๋ธ๋ฆฟ ์ข๋ฃ ์ destory๊ฐ ์คํ๋๋ค.
  
## ๐ฎ ๊ฐ๋
### 1. Filter(ํํฐ)
- ์์ฒญ๊ณผ ์๋ต์ ๊ฑฐ๋ฅธ๋ค ์ ์ ํ๋ ์ญํ ์ ํ๋ค.
- ์๋ธ๋ฆฟ ํํฐ๋ DispatcherServlet ์ด์ ์ ์คํ์ด ๋๋๋ฐ ํํฐ๊ฐ ๋์ํ๋๋ก ์ง์ ๋ ์์์ ์๋จ์์ ์์ฒญ๋ด์ฉ์ ๋ณ๊ฒฝํ๊ฑฐ๋,ย  ์ฌ๋ฌ๊ฐ์ง ์ฒดํฌ๋ฅผ ์ํํ  ์ ์๋ค.
- ์์์ ์ฒ๋ฆฌ๊ฐ ๋๋ ํ ์๋ต๋ด์ฉ์ ๋ํด์๋ ๋ณ๊ฒฝํ๋ ์ฒ๋ฆฌ๋ฅผ ํ  ์๊ฐ ์๋ค.
- ๋ณดํต web.xml์ ๋ฑ๋กํ๊ณ , ์ผ๋ฐ์ ์ผ๋ก ์ธ์ฝ๋ฉ ๋ณํ ์ฒ๋ฆฌ, XSS๋ฐฉ์ด ๋ฑ์ ์์ฒญ์ ๋ํ ์ฒ๋ฆฌ๋ก ์ฌ์ฉ๋๋ค.  
### ์คํ ๋ฉ์๋
1. init() - ํํฐ ์ธ์คํด์ค ์ด๊ธฐํ  
2. doFilter() - ์ /ํ ์ฒ๋ฆฌ  
3. destroy() - ํํฐ ์ธ์คํด์ค ์ข๋ฃ  
  
### 2. ์ธํฐ์ํฐ(Interceptor)
- ์์ฒญ์ ๋ํ ์์ ์ /ํ๋ก ๊ฐ๋ก์ฑ๋ค๊ณ  ๋ณด๋ฉด ๋๋ค.
- ํํฐ๋ ์คํ๋ง ์ปจํ์คํธ ์ธ๋ถ์ ์กด์ฌํ์ฌ ์คํ๋ง๊ณผ ๋ฌด๊ดํ ์์์ ๋ํด ๋์ํ๋ค. 
- ํ์ง๋ง ์ธํฐ์ํฐ๋ ์คํ๋ง์ DistpatcherServlet์ด ์ปจํธ๋กค๋ฌ๋ฅผ ํธ์ถํ๊ธฐ ์ , ํ๋ก ๋ผ์ด๋ค๊ธฐ ๋๋ฌธ์ ์คํ๋ง ์ปจํ์คํธ(Context, ์์ญ) ๋ด๋ถ์์ Controller(Handler)์ ๊ดํ ์์ฒญ๊ณผ ์๋ต์ ๋ํด ์ฒ๋ฆฌํ๋ค.
- **์คํ๋ง์ ๋ชจ๋  ๋น ๊ฐ์ฒด์ ์ ๊ทผํ  ์ ์๋ค.**
- ์ฌ๋ฌ๊ฐ ์ฌ์ฉ ๊ฐ๋ฅ
- ๋ก๊ทธ์ธ ์ฒดํฌ, ๊ถํ ์ฒดํฌ, ํ๋ก๊ทธ๋จ ์คํ์๊ฐ ๊ณ์ฐ์์, ๋ก๊ทธํ์ธ ๋ฑ
### ์คํ ๋ฉ์๋
1. preHandler() - ์ปจํธ๋กค๋ฌ ๋ฉ์๋๊ฐ ์คํ๋๊ธฐ ์ 
2. postHanler() - ์ปจํธ๋กค๋ฌ ๋ฉ์๋ ์คํ์ง ํ viewํ์ด์ง ๋ ๋๋ง ๋๊ธฐ ์ 
3. afterCompletion() - viewํ์ด์ง๊ฐ ๋ ๋๋ง ๋๊ณ  ๋ ํ
  
### 3. AOP
OOP๋ฅผ ๋ณด์ํ๊ธฐ ์ํด ๋์จ ๊ฐ๋
- ๊ฐ์ฒด ์งํฅ์ ํ๋ก๊ทธ๋๋ฐ์ ํ์ ๋ ์ค๋ณต์ ์ค์ผ ์ ์๋ ๋ถ๋ถ์ ์ค์ด๊ธฐ ์ํด ์ข๋จ๋ฉด(๊ด์ )์์ ๋ฐ๋ผ๋ณด๊ณ  ์ฒ๋ฆฌํ๋ค.
- ๋ก๊น, ํธ๋์ญ์, ์๋ฌ ์ฒ๋ฆฌ ๋ฑ ๋น์ฆ๋์ค๋จ์ ๋ฉ์๋์์ ์กฐ๊ธ ๋ ์ธ๋ฐํ๊ฒ ์กฐ์ ํ๊ณ  ์ถ์ ๋ ์ฌ์ฉํฉ๋๋ค.
- Interceptor๋ Filter์๋ ๋ฌ๋ฆฌ ๋ฉ์๋ ์ ํ์ ์ง์ ์ ์์ ๋กญ๊ฒ ์ค์ ์ด ๊ฐ๋ฅํ๋ค.
- **Interceptor์ Filter๋ ์ฃผ์๋ก ๋์์ ๊ตฌ๋ถํด์ ๊ฑธ๋ฌ๋ด์ผํ๋ ๋ฐ๋ฉด, AOP๋ ์ฃผ์, ํ๋ผ๋ฏธํฐ, ์ด๋ธํ์ด์ ๋ฑ ๋ค์ํ ๋ฐฉ๋ฒ์ผ๋ก ๋์ ์ง์  ๊ฐ๋ฅ**  
- AOP์ Advice์ HandlerInterceptor์ ๊ฐ์ฅ ํฐ ์ฐจ์ด๋ ํ๋ผ๋ฏธํฐ์ ์ฐจ์ด๋ค.
    - Advice์ ๊ฒฝ์ฐ JoinPoint๋ ProceedingJoinPoint ๋ฑ์ ํ์ฉํด์ ํธ์ถํ๋ค.
    - ๋ฐ๋ฉด HandlerInterceptor๋ Filter์ ์ ์ฌํ๊ฒ HttpServletRequest, HttpServletResponse๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ์ฌ์ฉํ๋ค.

### AOP ์ฉ์ด
|์ด๋ฆ|์ค๋ช|
|:---|:---|
|JoinPoint|๊ณตํต ๊ธฐ๋ฅ์ด ์ฝ์๋์ด ๋์๋  ์ ์๋ ์์น|
|PointCut|์กฐ์ธํฌ์ธํธ ์ค์์ ์คํ ํ๋ ค๋ ์์น๋ฅผ ๊ฒฐ์ |
|Advice|์ค์ ๋ก ์ ์ฉํ๋ ๊ธฐ๋ฅ(๋ก๊ทธ, ํธ๋์ญ์, ์ธ์ฆ)์ผ๋ก ์ ์ฉํ๋ ์์ ์ ๊ฐ์ง๊ณ  ์๋ค.|
|Target|๊ณตํต๊ธฐ๋ฅ์ด ์ ์ฉ๋  ๋์ ํด๋์ค ๊ฐ์ฒด|
|Weaving|Advice๋ฅผ ๋น์ฆ๋์ค ๋ก์ง ์ฝ๋์ ์ฝ์ํ๋ ๊ฒ|
|Aspect|์ฌ๋ฌํด๋์ค๋ ๊ธฐ๋ฅ์ ์ฌ์ฉ๋๋ ๊ด์ฌ์ฌ๋ฅผ ๋ชจ๋ํํจ(ํฌ์ธํธ ์ปท + ์ด๋๋ฐ์ด์ค) ex) ํธ๋์ญ์ ๊ด๋ฆฌ ๊ธฐ๋ฅ|
  
### Advice ์ข๋ฅ
|์ด๋ฆ|์ค๋ช|
|:---|:---|
|@Before|๋ฏธ๋ฆฌ ์ ์ํ Pointcut ์ง์์ ์ ์ํ|
|@AfterReturning|๋ฏธ๋ฆฌ ์ ์ํ Pointcut์์ return์ด ๋ฐ์ํ ํ์ ์ํ|
|@AfterThrowing|๋ฏธ๋ฆฌ ์ ์ํ Pointcut์์ ์์ธ๊ฐ ๋ฐ์ํ  ๊ฒฝ์ฐ ์ํ|
|@After|๋ฏธ๋ฆฌ ์ ์ํ Pointcut์์ ์์ธ ๋ฐ์ ์ฌ๋ถ์ ์๊ด์์ด ๋ฌด์กฐ๊ฑด ์ํ|
|@Around|๋ฏธ๋ฆฌ ์ ์ํ Pointcut์ ์ ,ํ์ ํ์ํ ๋์์ ์ํ|
  
### ๋ช์์ execution
- execution(AspectJ ํํ์)
> execution(์ ํ์ํจํด? ๋ฆฌํดํ์ํจํด? ์ด๋ฆํจํด(ํ๋ผ๋ฏธํฐํจํด)) ? - ์๋ต๊ฐ๋ฅ  

### AspectJ ํํ์
|์์ผ๋์นด๋|์ฐ์ฐ์|
|:---|:---|
|*|1๊ฐ์ ๋ชจ๋  ๊ฐ์ ํํ, ๋งค๊ฐ๋ณ์ ์ฌ์ฉ(1๊ฐ์ ํ๋ผ๋ฏธํฐ), ํจํค์ง ์ฌ์ฉ(1๊ฐ์ ํจํค์ง)|
|..|0๊ฐ ์ด์์ ๊ฐ์ ํํ|
|+|์ฃผ์ด์ง ํ์์ ์์ ํด๋์ค๋ ์ธํฐํ์ด์ค๋ฅผ ํํ|  
  
### AspectJ ํํ์ ์ ์ฉ ์์
|์ฌ์ฉ|์ค๋ช|
|:---|:---|
|execution(public * *(..)) |public ๋ฉ์๋ ์ ํ|
|execution(* set*(..))|์ด๋ฆ์ด set์ผ๋ก ์์ํ๋ ๋ชจ๋  ๋ฉ์๋|
|execution(* get*(..)) |์ด๋ฆ์ด get์ผ๋ก ์์ํ๋ ๋ชจ๋  ๋ฉ์๋|
|execution(* main(..))|์ด๋ฆ์ด main์ธ ๋ฉ์๋|
|execution(* com.ssafy..(..))|com.ssafy ํจํค์ง์ ์๋ ํด๋์ค์ ๋ชจ๋  ๋ฉ์๋|

### API ํด๋์ค
- JoinPoint
    - Object getTarget() ํต์ฌ ๊ธฐ๋ฅ ํด๋์ค ๊ฐ์ฒด
    - Signature getSignature() ํต์ญ๊ธฐ๋ฅ ํด๋์ค์ ๋ฉ์๋
- ProceedingJointPoint
    - Signature getSignature() ํต์ฌ๊ธฐ๋ฅ ํด๋์ค์ ๋ฉ์๋
    - proceed() ํต์ฌ๊ธฐ๋ฅ ํด๋์ค์ ๋ฉ์๋ ์คํ
  
### AOP ์ ์ฉ
- ํด๋์ค ์์ชฝ์ @Aspect ์ ์ธ
- ๋ฉ์๋ ์์ชฝ์ advice ์ ์ธ
  

#### ์ฐธ๊ณ ์๋ฃ
---
##### https://goddaehee.tistory.com/154
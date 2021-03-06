# ๐ Spring Async

## @Async
Async Annotation์ Spring์์ ์ ๊ณตํ๋ Thread Pool์ ํ์ฉํ๋ ๋น๋๊ธฐ ๋ฉ์๋ ์ง์ Annotation์ด๋ค.

## ๊ธฐ์กด์ Java์์์ ๋น๋๊ธฐ ๋ฐฉ์์ผ๋ก ๋ฉ์๋๋ฅผ ๊ตฌํํ  ๋
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class JavaAsync {

    static ExecutorService executorService = Executors.newFixedThreadPool(5);

    public void asyncMethod(final String message) throws Exception {
        executorService.submit(new Runnable() { // Runnable
            @Override
            public void run() {
                // do something
            }            
        });
    }
}
```
**java.util.concurrent.ExecutorService**์ ํ์ฉํด์ ๋น๋๊ธฐ ๋ฐฉ์์ method๋ฅผ ์ ์ ํ  ๋๋ง๋ค ์์ ๊ฐ์ด **Runnable**์ **run()**์ ์ฌ๊ตฌํํด์ผ ํ๋ ๋ฑ ๋์ผํ ์์๋ค์ ๋ฐ๋ณต์ด ์ฆ์๋ค.

## @Async ์ฌ์ฉ
**@Async** Annotation์ ํ์ฉํ๋ฉด ์์ฝ๊ฒ ๋น๋๊ธฐ ๋ฉ์๋ ์์ฑ์ด ๊ฐ๋ฅํ๋ค.  
๋ง์ฝ **Spring Boot**์์ ๊ฐ๋จํ ์ฌ์ฉ ๋ฐฉ๋ฒ
1. **Application** Class์ **@EnableAsync** Annotation์ ์ถ๊ฐํ๊ณ 
```java
@EnableAsync
@SpringBootApplication
public class SpringBootApplication {
    ...
}
```
2. ๋น๋๊ธฐ๋ก ์๋ํ๊ธธ ์ํ๋ method์์ **@Async** Annotation์ ๋ถ์ฌ์ฃผ๋ฉด ์ฌ์ฉํ  ์ ์๋ค.
```java
public class AsyncTest{
    @Async
    public void asyncMethod() 
}
```


### ๋ณธ์ธ ๊ฐ๋ฐ ํ๊ฒฝ์ ๋ง๊ฒ Customizeํ๊ธฐ ์ํด์๋ **AsyncConfigurerSupport**๋ฅผ ์์๋ฐ๋ Class๋ฅผ ์์ฑํ๋ ๊ฒ์ด ์ข๋ค.

## AsyncConfigurerSupport
```java
import java.util.concurrent.Executor;

import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.AsyncConfigurerSupport;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {
    
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(30);
        executor.setQueueCapacity(50);
        executor.setThreadNamePrefix("DDAJA-ASYNC-");
        executor.initialize();
        return executor;
    }
}
// ์์ ๊ฐ์ด ์ค์ ํ๋ค๋ฉด ์ต์ด 5๊ฐ์ ์ค๋ ๋์์ ์ฒ๋ฆฌํ๋ค๊ฐ ์ฒ๋ฆฌ ์๋๊ฐ ๋ฐ๋ฆด ๊ฒฝ์ฐ 100๊ฐ ์ฌ์ด์ฆ queue์์ ๋๊ธฐํ๊ณ  ๊ทธ ๋ณด๋ค ๋ง์ ์์ฒญ์ด ๋ฐ์ํ  ๊ฒฝ์ฐ ์ต๋ 30๊ฐ ์ค๋ ๋๊น์ง ์์ฑํด์ ์ฒ๋ฆฌํ๊ฒ ๋ฉ๋๋ค.
```
### ์ค์  ์์
- **@Configuration** : Spring ์ค์  ๊ด๋ จ Class๋ก @Component ๋ฑ๋ก๋์ด Scanning ๋  ์ ์๋ค.
- **@EnableAsync** : Spring method์์ ๋น๋๊ธฐ ๊ธฐ๋ฅ์ ์ฌ์ฉ๊ฐ๋ฅํ๊ฒ ํ์ฑํ ํ๋ค.
- **CorePoolSize** : ๊ธฐ๋ณธ ์คํ ๋๊ธฐํ๋ Thread์ ์**
- **MaxPoolSize** : ๋์ ๋์ํ๋ ์ต๋ Thread์ ์
- **QueueCapacity** : MaxPoolSize ์ด๊ณผ ์์ฒญ์์ Thread ์์ฑ ์์ฒญ์,
ํด๋น ์์ฒญ์ Queue์ ์ ์ฅํ๋๋ฐ ์ด๋ ์ต๋ ์์ฉ ๊ฐ๋ฅํ Queue์ ์,
Queue์ ์ ์ฅ๋์ด์๋ค๊ฐ Thread์ ์๋ฆฌ๊ฐ ์๊ธฐ๋ฉด ํ๋์ฉ ๋น ์ ธ๋๊ฐ ๋์
- **ThreadNamePrefix** : ์์ฑ๋๋ Thread ์ ๋์ฌ ์ง์ 

  
## ์ฃผ์ ์ฌํญ
1. private method๋ ์ฌ์ฉ ๋ถ๊ฐ, public method๋ง ์ฌ์ฉ ๊ฐ๋ฅ
2. self-invocation(์๊ฐ ํธ์ถ) ๋ถ๊ฐ, ์ฆ innermethod๋ ์ฌ์ฉ ๋ถ๊ฐ
3. QueueCapacity ์ด๊ณผ ์์ฒญ์ ๋ํ ๋น๋๊ธฐ method ํธ์ถ ์ ๋ฐฉ์ด ์ฝ๋ ์์ฑ

์ฐธ๊ณ ์๋ฃ  

## Async ๋์ ๋ฐฉ์
### AOP๊ฐ ์ ์ฉ๋์ด Spring Context์์ ๋ฑ๋ก๋ Bean Object์ method๊ฐ ํธ์ถ ๋  ์์, Spring์ด ํ์ธํ  ์ ์๊ณ  @Async๊ฐ ์ ์ฉ๋ method์ ๊ฒฝ์ฐ Spring์ด method๋ฅผ ๊ฐ๋ก์ฑ ๋ค๋ฅธ Thread์์ ์คํ ์์ผ์ฃผ๋ ๋์ ๋ฐฉ์์ด๋ค.  
### ์ด ๋๋ฌธ์ Spring์ด ํด๋น @Async method๋ฅผ ๊ฐ๋ก์ฑ ํ, ๋ค๋ฅธ Class์์ ํธ์ถ์ด ๊ฐ๋ฅํด์ผ ํ๋ฏ๋ก, private method๋ ์ฌ์ฉํ  ์ ์๋ ๊ฒ์ด๋ค.  
### ๋ํ Spring Context์ ๋ฑ๋ก๋ Bean์ method์ ํธ์ถ์ด์ด์ผ Proxy ์ ์ฉ์ด ๊ฐ๋ฅํ๋ฏ๋ก, inner method์ ํธ์ถ์ Proxy ์ํฅ์ ๋ฐ์ง ์๊ธฐ์ self-invocation์ด ๋ถ๊ฐ๋ฅํ๋ค.

## QueueCapacity ์ด๊ณผ ์์ฒญ ๋ฐฉ์ด ์ฝ๋ ์์ฑ
์ด๋ฒ์ **AsyncConfig**์์ **PoolSize**์ **QueueCapacity**๋ฅผ ์ค์ฌ๋ณด๊ณ  ์ ์ฝ๋๋ฅผ ๋ค์ ์คํํด๋ณด์.
```java
@Service
public class TestService {
    @Async
    public void asyncMethod(int i) {
        try {
            Thread.sleep(500);
            log.info("[AsyncMethod]"+"-"+i);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }
    }
}
@AllArgsConstructor
@Controller
public Class TestController {

    private TestService testService;

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        for(int i=0; i<50; i++) {
            testService.asyncMethod(i);
        }
        return "";
    } 
}
@Configuration
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {
    
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(10);
        executor.setThreadNamePrefix("DDAJA-ASYNC-");
        executor.initialize();
        return executor;
    }
}
// queued ๋ tasks = 10๊ฐ๋ก ์ต๋ ์์ฉ ๊ฐ๋ฅํ Thread Pool ์์ QueueCapacity ๊น์ง ์ด๊ณผ๋ ์์ฒญ์ด ๋ค์ด์ค์,
// Task๋ฅผ Rejectํ๋ Exception์ด ๋ฐ์ํ์๋ค.
// i = 0 ~49 ์ฆ ๊ธฐ์กด์๋ 50๊ฐ์ QueueSize๋ฅผ ์ฌ์ฉํ์ฌ ์งํํด๋ ์ค๋ฅ๊ฐ ์์์ง๋ง ํ์ฌ๋ 10๊ฐ์ QueueSize๋ฅผ ์ฌ์ฉํ๊ณ  MaxPoolSize 10์ผ๋ก ์ธํด ์์์ด ๋ฐ๋ ค MaxPoolSize์ setQueueCapacity๊น์ง ์ด๊ณผ๋ ์์ฒญ์ด ๋ค์ด์์
// Task๋ฅผ Reject๋ฅผ ํ๋ Exception์ด ๋ฐ์ํ์๋ค.
```

## TaskRejectedException Handling
```java
@AllArgsConstructor
@Controller
public Class TestController {

    private TestService testService;

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        try {
            for(int i=0; i<50; i++) {
                testService.asyncMethod(i);
        } catch (TaskRejectedException e) {
            // ....
        }
        return "";
    }
    
}
```
---
[gillog](https://velog.io/@gillog/Spring-Async-Annotation%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%A9%94%EC%86%8C%EB%93%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
[dzone](https://dzone.com/articles/effective-advice-on-spring-async-part-1)
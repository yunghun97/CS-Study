# π€  RequiredArgsConstructor (μμ±μ μ£Όμ)

## @RequiredArgsConstructor
μ΄ μ΄λΈνμ΄μμ μ΄κΈ°ν λμ§μμ **final**νλλ, **@NonNull**μ΄ λΆμ νλμ λν΄ μμ±μλ₯Ό μμ±ν΄ μ€λλ€. μ£Όλ‘ μμ‘΄μ± μ£Όμ(Dependency Injection) νΈμμ±μ μν΄μ μ¬μ©νλ€.

## μμ‘΄μ± μ£Όμ μ’λ₯
1. Constructor(μμ±μ)
```java
public class Example{
    private final AService aService;
    @Autowired
    public Example(AService aService){
        this.aService = aService;
    }
}
```
2. Setter
```java
public class Example{
    private AService aService;
    private BService bService;

    @Autowired
    public void setAService(AService aService){
        this.aService = aService;
    }
    @Autowired
    public void setBService(BService bService){
        this.bService = bService;
    }
}
```
3. Field
```java
public class Example{
    @Autowired
    private AService aService;
}
```

## @RequiredArgsConstructor μ΄λΈνμ΄μμ μ¬μ©ν μμ±μ μ£Όμ
μμ±μμ λ¨μ μ μμ μμ±μ μ½λλ₯Ό λ§λ€κΈ° λ²κ±°λ‘­λ€. μ΄λ₯Ό lombokμ μ¬μ©νμ¬ κ°λ¨ν λ°©λ²μΌλ‘ μμ±μ μ£Όμ λ°©μμ μ½λ© κ°λ₯
### @RequiredArgsConstructor
**final**μ΄ λΆκ±°λ **@NotNull**μ΄ λΆμ νλμ μμ±μλ₯Ό μλ μμ±ν΄μ£Όλ lombok μ΄λΈνμ΄μ
1. κΈ°μ‘΄μ filed μ£Όμ λ°©μ
```java
@Service
public class Test{
    @Autowired
    private AService aService;
}
```
2. @RequiredArgsConstructor λ₯Ό νμ©ν μμ±μ μ£Όμ
```java
@Service
@RequiredArgsConstructor
public class Test{
    private final AService aService;
}
```
3. @RequiredArgsConstructor λ₯Ό μ¬μ©νμ§ μμ μ
```java
public class Example{
    private final AService aService;
    @Autowired
    public Example(AService aService){
        this.aService = aService;
    }
}
```
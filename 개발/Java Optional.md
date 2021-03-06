# ๐ Java Optional

### Java 8 ๋ถํฐ ์ง์ํ๋ ํด๋์ค

## ๐ฎ NPE(NullPointerException)
- NPE๋ฅผ ํผํ๊ธฐ์ํด์๋ null ๊ฒ์ฌ ๋ก์ง์ ์ถ๊ฐํด์ค์ผ ํ๋๋ฐ ์ด๋ฅผ ๋ง์ null ๊ฒ์ฌ๋ฅผ ์งํํ๋ฉด ์ฝ๋๊ฐ ๋ณต์กํด์ง๊ณ  ๋ฒ๊ฑฐ๋๊ณ  ๋๋ค.

```java
List<Integer> list = getList();
// getList์์์ return ๊ฐ์ด null์ด๋ผ๋ฉด NPE๊ฐ ๋ฐ์๋๋ค.
System.out.print(list.size());

if(list!=null){ // null ์ฒดํฌ
    System.out.print(list.size());
}
```

## ๐ Optional
- Java 8 ์์๋ Optional<T> ํด๋์ค๋ฅผ ์ฌ์ฉํด NPE๋ฅผ ๋ฐฉ์งํ  ์ ์๋๋ก ๋์์ค๋ค. 
- Optional<T>๋ null์ด ์ฌ ์ ์๋ ๊ฐ์ ๊ฐ์ธ๋ Wrapper ํด๋์ค๋ก, ์ฐธ์กฐํ๋๋ผ๋ NPE๊ฐ ๋ฐ์ํ์ง ์๋๋ก ๋์์ค๋ค. 
- Optional ํด๋์ค๋ ์๋์ ๊ฐ์ value์ ๊ฐ์ ์ ์ฅํ๊ธฐ ๋๋ฌธ์ ๊ฐ์ด null์ด๋๋ผ๋ ๋ฐ๋ก NPE๊ฐ ๋ฐ์ํ์ง ์์ผ๋ฉฐ, ํด๋์ค์ด๊ธฐ ๋๋ฌธ์ ๊ฐ์ข ๋ฉ์๋๋ฅผ ์ ๊ณตํด์ค๋ค.

```java
public final class Optional<T>{
    private final T value;
    ...
}
```

## ๐ Optional ์์ฑํ๊ธฐ
> value๋ String ๋ณ์๋ผ ๊ฐ์ 
1. ๋ฌด์กฐ๊ฑด null์ด ์๋ ๊ฐ์ Optional ์์ฑํ  ๋
```java
Optional<String> optional = Optional.of(value); 
// value ๋ณ์์ ๊ฐ์ด null์ธ ๊ฒฝ์ฐ์ NPE ๋ฐ์
```
2. null์ด์ฌ๋ ์๊ด์ด ์๋ ๊ฒฝ์ฐ
```java
Optional<String> optional = Optional.ofNullable(value);
// value ๋ณ์์ ๊ฐ์ด null์ผ ์ ์์ผ๋ฉฐ null์ผ ๊ฒฝ์ฐ Optional.empty()๊ฐ return ๋๋ค.
```
3. ๋น Optional ๊ฐ์ฒด ์์ฑ
```java
Optional<String> optional = Optional.empty();
// Optional ๊ฐ์ฒด ์์ฒด๋ ์์ง๋ง ๋ด๋ถ์์ ๊ฐ๋ฆฌํค๋ ์ฐธ์กฐ๊ฐ ์๋ ๊ฒฝ์ฐ์ด๋ฉฐ Optional.empty() ๊ฐ์ฒด๋ ๋ฏธ๋ฆฌ ์์ฑ๋์ด ์๋ ์ฑ๊ธํด ์ธ์คํด์ค๋ค.
```
```java
public final class Optional<T> {
    /**
     * Common instance for {@code empty()}.
     */
    private static final Optional<?> EMPTY = new Optional<>();

    /**
     * If non-null, the value; if null, indicates no value is present
     */
    private final T value;
    private Optional() {
        this.value = null;
    }
}
```
- static ๋ณ์๋ก EMPTY ๊ฐ์ฒด๋ฅผ ๋ฏธ๋ฆฌ ์์ฑํด์ ๊ฐ์ง๊ณ  ์๋ค. ์ด๋ฌํ ์ด์ ๋ก ๋น ๊ฐ์ฒด๋ฅผ ์ฌ๋ฌ ๋ฒ ์์ฑํด์ค์ผ ํ๋ ๊ฒฝ์ฐ์๋ 1๊ฐ์ EMPTY ๊ฐ์ฒด๋ฅผ ๊ณต์ ํจ์ผ๋ก์จ ๋ฉ๋ชจ๋ฆฌ๋ฅผ ์ ์ฝํ๊ณ  ์๋ค.

## ๐ Optional ์ฌ์ฉ๋ฒ - ์ค๊ฐ ์ฒ๋ฆฌ
- Optional ๊ฐ์ฒด๋ฅผ ๊ฐ์ ธ์์ ์ค๊ฐ์ ์ฒ๋ฆฌํ๊ณ  ๋ค์ Optional ๊ฐ์ฒด๋ฅผ ๋ฐํํ๋ ๋ฉ์๋๋ค์ด ์๋ค. ์ค๊ฐ์ฒ๋ฆฌ ๋ฉ์๋๋ค์ ์ฐ์ด์ด ์ด์ด๋ถ์ด ์ํ๋ ๋ก์ง์ ๋ฐ๋ณตํด์ ์ฌ์ฉํ  ์ ์๋ค.

1. filter()
- filter ๋ฉ์๋์ ์ธ์๋ก ๋ฐ์ ๋๋ค์์ด ์ฐธ์ด๋ฉด Optional ๊ฐ์ฒด๋ฅผ ๊ทธ๋๋ก ํต๊ณผ์ํค๊ณ  ๊ฑฐ์ง์ด๋ฉด Optional.empty()๋ฅผ ๋ฐํํด์ ์ถ๊ฐ๋ก ์ฒ๋ฆฌ๊ฐ ์๋๋๋ก ํ๋ค.
```java
Optinal.of("ABCD").filter(v -> v.startsWith("AB")).orElse("Not AB");
// filter(๋ณ์ -> ๋ณ์.์กฐ๊ฑด)
// ์ ์ฝ๋๋ ์กฐ๊ฑด์ ๋ถํฉํ๋ฏ๋ก ABCD return ๋ถํฉํ์ง ์์ผ๋ฉด orElse๋ก ์ ์ Not AB๊ฐ return ๋๋ค.

Optional<String> opStr1 = Optional.of("first string");
Optional<String> opStr2 = Optional.of("second string");
Optional<String> filtered1 = opStr1.filter(s -> s.contains("first"));
Optional<String> filtered2 = opStr2.filter(s -> s.contains("first"));
filtered1.ifPresent(System.out::println); // Optional ๊ฐ์ฒด filtered1 ์ถ๋ ฅ
filtered2.ifPresent(System.out::println); // Optional.empty() ์ด๋ฏ๋ก ์๋ฌด๊ฒ๋ ์ถ๋ ฅ๋์ง ์๋๋ค.
```
  
2. map()
- Optional ๊ฐ์ฒด์ ๊ฐ์ ์ด๋ค ์์ ์ ๊ฐํด์ ๋ค๋ฅธ ๊ฐ์ผ๋ก ๋ณ๊ฒฝํ๋ ๋ฉ์๋
```java
Order order = getOrder(); // 

Optional.ofNullable(order)
			.map(Order::getMember)
			.map(Member::getAddress)
			.map(Address::getCity)
			.orElse("Gwon");
// ํน์ Order ๊ฐ์ฒด๊ฐ null์ธ ๊ฒฝ์ฐ๋ฅผ ๋๋นํ์ฌ of() ๋์ ์ ofNullable()์ ์ฌ์ฉํ์ต๋๋ค.
// map() ๋ฉ์๋์ ์ฐ์ ํธ์ถ์ ํตํด์ Optional ๊ฐ์ฒด๋ฅผ 3๋ฒ ๋ณํํ์์ต๋๋ค. ๋งค ๋ฒ ๋ค๋ฅธ ๋ฉ์๋ ๋ ํผ๋ฐ์ค๋ฅผ ์ธ์๋ก ๋๊ฒจ์ Optional์ ๋ด๊ธด ๊ฐ์ฒด์ ํ์์ ๋ฐ๊ฟ์ฃผ์์ต๋๋ค. (Optional<Order> -> Optional<Member> -> Optional<Address> -> Optional<String>)
// ๋ง๋ฌด๋ฆฌ ์์์ผ๋ก orElse() ๋ฉ์๋๋ฅผ ํธ์ถํ์ฌ ์ด ์  ๊ณผ์ ์ ํตํด ์ป์ Optional์ด ๋น์ด์์ ๊ฒฝ์ฐ, orElse() ๊ฐ์ด ์ธํ๋ฉ๋๋ค.
``` 

## ๐ฅ ๊ฐ์ returnํ๋ ๋ฉ์๋
- ์ค๊ฐ ๋ฉ์๋๋ค์ Optional ๊ฐ์ฒด๋ฅผ returnํด์ ๋ฉ์๋ ์ฒด์ธ์ผ๋ก ์ฌ์ฉํ  ์ ์๋ ๋ฐ๋ฉด, ๊ฐ์ returnํด์ ๋ฉ์๋ ์ฒด์ธ์ ๋๋ด๋ ๋ฉ์๋๋ค๋ ์๋ค.  

1. isPresent()
- Optional ๊ฐ์ฒด์ ๊ฐ์ด ์์ผ๋ฉด true ์์ผ๋ฉด false๋ฅผ return ํด์ค๋ค.
```java
Optional<String> option = Optional.ofNullable("asd");
System.out.println(option.isPresent()); // true
```  
  
2. ifPresent()
- ifPresent() ๋ฉ์๋๋ ๋๋ค์์ ์ธ์๋ก ๋ฐ์, ๊ฐ์ด ์กด์ฌํ  ๋ ๊ทธ ๊ฐ์ ๋๋ค์์ ์ ์ฉํด์ค๋ค. ๋ง์ฝ Optoinal ๊ฐ์ฒด์ ๊ฐ์ด ์๋ค๋ฉด ๋๋ค์์ด ์คํ๋์ง ์๋๋ค.
```java
Optional<String> option = Optional.ofNullable("ifTest");
option.ifPresent(v -> System.out.print(v)); //ifTest ์ถ๋ ฅ
```

3. get()
- Optional ๊ฐ์ฒด๊ฐ ๊ฐ์ง๊ณ  ์๋ value ๊ฐ์ ๊บผ๋ด์จ๋ค. ๋ง์ฝ Optional ๊ฐ์ฒด์ ๊ฐ์ด ์๋ค๋ฉด, NoSuchElementException์ด ๋ฐ์๋๋ค.
```java
Optional<String> option = Optional.ofNullable("getTest");
System.out.print(option.get()); // getTest ์ถ๋ ฅ
```  
  
4. orElse()
- ์ต์ข์ ์ผ๋ก orElse๊ฐ ์คํ๋  ๋ Optional ๊ฐ์ฒด๊ฐ ๋น์ด์์๋ค๋ฉด, orElse() ๋ฉ์๋์ ์ง์ ๋ ๊ฐ์ด ๊ธฐ๋ณธ๊ฐ์ผ๋ก return ๋๋ค.
```java
Optional<String> option;
System.out.println(option); // Optional.empty
System.out.println(option.orElse("Else Test")); // Else Test
```
5. orElseGet()
- ์ค๊ฐ์ฒ๋ฆฌ ๋ฉ์๋๋ค์ ๊ฑฐ์น๋ฉด์ ํน์ ์๋ Optional ๊ฐ์ฒด๊ฐ ๋น์ด์์๋ค๋ฉด, orElseGet() ๋ฉ์๋์ ์ธ์๋กค ์๋ ฅ๋์ด ์๋ Supplier ํจ์๋ฅผ ์ ์ฉํ์ฌ ๊ฐ์ฒด๋ฅผ ์ป์ด์จ๋ค.
```java

Optional<String> test = Optional.of("XYZ").filter(v -> v.startsWith("AB")).orElseGet(() -> "NotAB"); // NotAB return

// orElse ์ฐจ์ด์  - ํ์คํธ 1
String elseName = Optional.ofNullable("HoHo").orElse(test());
System.out.println(elesName);
String elseGetName = Optional.ofNullable("NoNo").orElseGet(()->test());
System.out.println(elseGetName);

private String test() {
    System.out.println("Test Print");
    return "i`m Test";
}
// ๊ฒฐ๊ณผ
// Test Print
// HoHo
// NoNo

// orElse ์ฐจ์ด์  - ํ์คํธ 2
String a = null;
String elseName = Optional.ofNullable(a).orElse(test());
System.out.println(elesName);
String elseGetName = Optional.ofNullable(a).orElseGet(()->test());
System.out.println(elseGetName);

private String test() {
    System.out.println("Test Print");
    return "i`m Test";
}
// ๊ฒฐ๊ณผ
// Test Print
// i`m Test
// Test Print
// i`m Test
```
> ๊ฒฐ๋ก  : **orElse**์ ๋งค๊ฐ๋ณ์๋ก ๋ฉ์๋๊ฐ ๋ฃ์ผ๋ฉด Optional์ ๊ฐ์ฒด์ ๊ฐ์ด null์ด๋  ์๋๋  ํด๋น ๋ฉ์๋๋ฅผ ๋ฌด์กฐ๊ฑด ์คํํ๋ค. -> orElse๋ฌธ์ Value๋ ๋ฉ๋ชจ๋ฆฌ์์ ์กด์ฌํ๋ค๊ณ  ๊ฐ์ ํ๊ธฐ ๋๋ฌธ์  
**orElseGet**๋ null์ผ ๊ฒฝ์ฐ Supplier๋ฅผ ํธ์ถํ๋ ๊ฒ์ด๋ฏ๋ก null์ด ์๋๋ฉด Supplier๋ฅผ ํธ์ถํ์ง ์๋๋ค.  
  
6. orElseThrow()
- ํด๋น ๋ฉ์๋์ ๋์ฐฉํ์ ๋ Optional ๊ฐ์ฒด๊ฐ ๋น์ด์์๋ค๋ฉด, Supplier ํจ์๋ฅผ ์ฑํํด ์์ธ๋ฅผ ๋ฐ์์ํจ๋ค. 
```java
Optional<String> option = Optional.empty();
option.orElseThrow(()->new NoClassDefFoundError()); // NoclassDefFoundError ์๋ฌ ๋ฐ์
option.orElseThrow(IndexOutOfBoundsException::new); // IndexOutOfBoundsException ์๋ฌ ๋ฐ์

```
  
## ๐งต Java9์์ ์ถ๊ฐ๋ ๋ฉ์๋๋ค
1. or()
- ์ค๊ฐ์ฒ๋ฆฌ ๋ฉ์๋๋ก OrElse(), orElseGet()๊ณผ ๋น์ทํ์ง๋ง Optional ๊ฐ์ฒด๋ฅผ return ํ๋ค.
- ๋ฉ์๋ ์ฒด์ธ ์ค๊ฐ์์ Optional empty()๊ฐ ๋์์ ๋, Optional.empty()๋์  ๋ค๋ฅธ Optional๊ฐ์ฒด๋ฅผ ๋ง๋ค์ด์ ๋ค์ชฝ์ผ๋ก ๋๊ฒจ์ค๋ค.
```java
String a = null;        
System.out.print(Optional.ofNullable(a).or(()-> Optional.ofNullable("orCheck")).orElse("Null check")); 
// orCheck ์ถ๋ ฅ
```
2. ifPresentOrElse()
- ์ต์ข์ ์ผ๋ก ๊ฐ์ ๋ฐํํ๋ ๋ฉ์๋๋ค. ifPresent() ๋ฉ์๋์ ์ ์ฌํ์ง๋ง ์ธ์๋ฅผ ํ๋ ๋ ๋ฐ๋๋ค. 
- ์ฒซ๋ฒ ์งธ ์ธ์๋ก ๋ฐ์ ๋๋ค์์ Optional ๊ฐ์ฒด์ ๊ฐ์ด ์กด์ฌํ๋ ๊ฒฝ์ฐ ์คํ๋๋ค. 
- ๋๋ฒ์งธ ์ธ์๋ก ๋ฐ์ ๋๋ค์์ Optional ๊ฐ์ฒด๊ฐ ๋น์ด์์ ๋ ์คํ๋๋ค.
```java
Optional.ofNullable("test").ifPresentOrElse(value -> System.out.println(value+"Not empty"), () -> System.out.println("null"));
// test Not empty ์ถ๋ ฅ
```
3. stream()
- ์ค๊ฐ์ฒ๋ฆฌ ์ฐ์ฐ์๋ก Java8์์ Optional ๊ฐ์ฒด๊ฐ ๋ฐ๋ก ์คํธ๋ฆผ ๊ฐ์ฒด๋ก ์ ํ ๋์ง ์์ ๋ถํธํ๋ ๋ถ๋ถ์ ํด์์์ผ ์ค๋๋ค.
```java
// ๋ฉ์๋ ์๊ทธ๋์ฒ
public Stream<T> stream();
// ์์ 
List<String> result = List.of(1, 2, 3, 4)
    .stream()
    .map(val -> val % 2 == 0 ? Optional.of(val) : Optional.empty())
    .flatMap(Optional::stream)
    .map(String::valueOf)
    .collect(Collectors.toList());
System.out.println(result); // print '[2, 4]'

// ์์ ๋ ๋ฆฌ์คํธ์์ ์ผ๋ถ ๊ฐ๋ง ์ถ์ถํ๊ณ  ์คํธ๋ฆผ์ผ๋ก ๋ณํํ ๋ค ๋ค์ ๋ฆฌ์คํธ๋ก ์์งํ๋ ๊ณผ์ ์ ๋ณด์ฌ์ค๋๋ค.
```

## ๐ Java 10
1. orElseThrow()
- Java8์ orElseThrow์ ๋์ผํ์ง๋ง ์ธ์๋ฅผ ๋ฐ์ง ์๋๋ค๋ ์ฐจ์ด๊ฐ ์๋ค.
```java
Optional<String> option = Optional.empty();
option.orElseThrow(()->new NoClassDefFoundError()); // NoclassDefFoundError ์๋ฌ ๋ฐ์
option.orElseThrow(); // NoSuchElementException: No value present
```
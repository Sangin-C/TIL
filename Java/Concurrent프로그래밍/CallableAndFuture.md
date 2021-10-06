# Callable And Future

---
- Callable은 Runnable과 거의 유사하지만 어떤 결과를 return할 수 있다.
- return 값은 Future<>이다.
---

- Future는 작업의 현재 상태나 결과를 조회할 수 있다.   
- Future는 .get()을 만난 순간 Thread가 끝나 값을 가져올때까지 기다린다. 이걸 블록킹 콜이라고 부른다.
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();

Callable<String> callable = () -> {
    Thread.sleep(2000L);
    return "Hello";
};

//상태나 결과 조회
Future<String> future = executorService.submit(callable);
System.out.println("Started!");

//get을 만난 순간 기다린다. ( 블록킹 콜 )
future.get();
System.out.println("End!");
```

### .isdone()
- 현재 상태를 알 수 있다.   

작업(Thread)가 끝났으면 true, 아직 실해중이라면 false를 return한다.
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();

Callable<String> callable = () -> {
    Thread.sleep(2000L);
    return "Hello";
};

Future<String> future = executorService.submit(callable);

future.isDone();

```
---
### .cancel()
- 진행중인 작업(Thread)를 중단시킬 수 있다. 
  
true = 중단 , false = 대기
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();

Callable<String> callable = () -> {
    Thread.sleep(2000L);
    return "Hello";
};

Future<String> future = executorService.submit(callable);

future.cancel();
```
---
### .invokeAll() .invokeAny()
- .invokeAll() - 여러 작업 동시에 실행하기
- .invokeAny() - 여러 작업 중 가장 먼저 응답이 오면 끝내기

Callable을 이용해 여러 작업(Thread)를 뭉쳐서 한꺼번에 보낼 수 있다.
```java
Callable<String> callable1 = () -> {
    Thread.sleep(1000L);
    return "callable1";
};
Callable<String> callable2 = () -> {
    Thread.sleep(2000L);
    return "callable2";
};
Callable<String> callable3 = () -> {
    Thread.sleep(3000L);
    return "callable3";
};

// invokeAll()은 제일 대기가 긴 callable3이 끝날때 까지 기다려야 3개의 결과값을 받아 올 수 있다.
List<Future<String>> invokeAll = executorService.invokeAll(Arrays.asList(callable1, callable2, callable3));
for (Future<String> f : invokeAll) {
System.out.println(f.get());
}

// invokeAny()는 결과값이 모두 같다고 가정을했을때 replication됏다고 가정을하고 가장 빨리 넘어오는 값의 결과를 받을수 있다.
String invokeAny = executorService.invokeAny(Arrays.asList(callable1, callable2, callable3));
System.out.println(invokeAny);
```
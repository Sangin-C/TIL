# CompletableFuture

---
- CompletableFuture은 자바에서 비동기 프로그래밍을 가능케하는 인터페이스이다.
- 기존에 Future로도 가능했지만, 여러 힘든일들이 많았다.
- Future는 외부에서 완료 시킬 수 없고, 블록킹 콜(.get())을 호출하지 않고서는 작업이 끝났을 때 콜백을 실행 할 수 없다.
- 여러 Future들을 조합하여 사용하는것도 어렵고, 예외 처리 API도 제공해주지 않는다.
- ThreadPool을 생성하지도 않고 어떻게 별도의 여러 Thread에서 동작이 가능한가??? 
  - ForkJoinPool때문에 가능하다.
  - JAVA7에서 도입된것인데, ForkJoinPool은 Executor를 구현한 구현체중 하나이다.
- Executor를 사용하지 않아도 내부적으로 FormJoinPool에 있는 commonPool을 사용하게 된다.
---
### CompletableFuture Interface
- CompletableFuture을 사용하면 Executors를 명시하지않고 그냥 바로 사용가능하다.
.get()메서드를 호출해야하는건 똑같다.
```java
CompletableFuture<String> completableFuture = new CompletableFuture<>();
completableFuture.complete("lokcy");
completableFuture.get();
```
---
## Thread 주요 메서드
### .runAsync()
- 리턴 값이 없을경우
```java
CompletableFuture<Void> runAsync = CompletableFuture.runAsync(() -> {
    System.out.println("Hello" + Thread.currentThread().getName());
});
runAsync.get();
```
---
### .supplyAsync()
- 리턴 값이 있는경우
```java
CompletableFuture<String> supplyAsync = CompletableFuture.supplyAsync(() -> {
    return "Hello";
});
supplyAsync.get();
```


---
## 콜백 사용 방법
Future는 get을 호출하기전에 콜백을 정의할 수 없엇지만, CompletableFuture은 정의할 수 있다.
### .thenApply()
- 입력값도 있고 리턴값도 있는 경우

값을 바꿀 수 있다. ( stream의 map과 유사 )
```java
CompletableFuture<String> thenApply = CompletableFuture.supplyAsync(() -> {
    return "Hello";
}).thenApply((s) -> {   //Future는 get을 호출하기전에 콜백을 정의할 수 없엇지만, CompletableFuture은 정의할 수 있다.
    System.out.println(Thread.currentThread().getName());
    return s.toUpperCase(Locale.ROOT);
});
thenApply.get();
```
---
### .thenApply()
- 입력값은 있지만 리턴이 없는 경우
```java
CompletableFuture<Void> thenAccept = CompletableFuture.supplyAsync(() -> {
    return "Hello";
}).thenAccept((s) -> {
    System.out.println(Thread.currentThread().getName());
    System.out.println(s.toUpperCase(Locale.ROOT));
});
thenAccept.get();
```
---
### .thenRuen()
- 입력값도없고 리턴도 없는 경우, 그냥 어떤 작업만 하면 되는 경우
```java
CompletableFuture<Void> thenRun = CompletableFuture.supplyAsync(() -> {
    return "Hello";
}).thenRun(() -> {
    System.out.println(Thread.currentThread().getName());
});
thenRun.get();
```
---
## 다양한 방법으로 사용하기
Thread Pool 사용, 조합, 예외처리 등 여러가지 작업을 할 수 있다.
### Executor를 사용해 만든 Thread Pool을 사용 방법
- 두번째 인자로 만든 Thread Pool을 입력하면 된다.
```java
ExecutorService ThreadPool = Executors.newFixedThreadPool(4);
CompletableFuture<Void> forkJoinPool = CompletableFuture.supplyAsync(() -> {
    return "Hello";
}, ThreadPool).thenRunAsync(() -> {
    System.out.println(Thread.currentThread().getName());
}, ThreadPool);
forkJoinPool.get();
```
---
## 여러 CompletableFuture 조합해서 사용하기
- 기존 Future만으로는 비동기적인 작업 2개를 연결하기에 쉽지 않았다. 
  - 왜?? 콜백을 줄수 없었기 때문이다.
```java
CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
    return "Hello";
});
CompletableFuture<String> world = CompletableFuture.supplyAsync(() -> {
    System.out.println("World " + Thread.currentThread().getName());
    return "World";
});
// 이전에는 hello.get() world.get()을 모두 호출한 뒤에 두개를 사용을 해야했다.
// .thenCompose()를 사용하면 바로 추가적인 작업이 가능하다.
hello.get();
world.get();
```
### .thenCompose()
- 의존성을 가지는 두 개의 작업을 하나의 Future로 받아 올 수 있다.

두 개의 작업이 의존성을 가지고 있을때 사용한다. EX) A다음에 B가 실행되어야한다.
```java
CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
    return "Hello";
});

CompletableFuture<String> thenCompose = hello.thenCompose(s -> getWorld(s));
thenCompose.get();

private static CompletableFuture<String> getWorld(String message) {
    return CompletableFuture.supplyAsync(() -> {
        System.out.println("World " + Thread.currentThread().getName());
        return "World";
    });
}
```
---
### .thenCombine()
- 두 개의 작업이 의존성을 가지고 있지 않을 경우, 각각의 작업이 따로 진행되도 되는경우 사용한다.
```java
CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
return "Hello";
});

CompletableFuture<String> world = CompletableFuture.supplyAsync(() -> {
    System.out.println("World " + Thread.currentThread().getName());
return "World";
});

CompletableFuture<String> thenCombine = hello.thenCombine(world, (h, w) -> {
    return h + " " + w;
});
thenCombine.get();
```
---

## 두 개 이상의 작업을 조합해야할때

### .anyOf()
- 여러 작업중 가장 빨리 끝나는 것을 가져온다.
```java
CompletableFuture<String> hello = CompletableFuture.supplyAsync(() -> {
    System.out.println("Hello " + Thread.currentThread().getName());
    return "Hello";
});

CompletableFuture<String> world = CompletableFuture.supplyAsync(() -> {
    System.out.println("World " + Thread.currentThread().getName());
    return "World";
});

CompletableFuture<Void> anyOf = CompletableFuture.anyOf(hello, world)
.thenAccept((result) -> System.out.println(result));
anyOf.get();
```
---

## 예외 처리
### .exceptionally()
- 예외를 처리할 수 있다.
```java
boolean throwsError = true;

CompletableFuture<String> exceoptionally = CompletableFuture.supplyAsync(() -> {
    if(throwsError){
        throw new IllegalStateException();
    }
    return "Hello";
}).exceptionally(ex -> {
    System.out.println(ex);
    return "Error";
});
exceoptionally.get();
```

### .handle()
- exceptionally()보다 더 일반적으로 쓰이는 메서드가 있는데 바로 .handle()이다.

handle()은 정상적으로 종료됐을때, 에러가 발생했을때 모두 사용할 수 있다.
```java
boolean throwsError = true;

CompletableFuture<String> handle = CompletableFuture.supplyAsync(() -> {
    if(throwsError){
        throw new IllegalStateException();
    }
    return "Hello";
    //첫번째는 정상적으로 종료 됐을때, 두번째는 에러가 발생했을때를 처리하기위해 BiFunction을 사용한다.
}).handle((result, ex) -> {
    if (ex != null) {
        System.out.println(ex);
        return "Error";
    }
    return result;
});
handle.get();
```
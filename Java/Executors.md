# Executors
- Thread나 Runnable을 직접 만들고 관리하는 작업을 Executor를 이용해 고수준의 Concurrent 프로그래밍을 가능하도록 API를 제공해준다.
- Executor 직접 사용하기보다 ExecutorService Interface를 사용한다.
---


## ExecutorService 인터페이스
ExecutorService는 Thread와 달리 어떤 작업을 시작하고 다음 작업이 들어올 때 까지 계속 대기를 하기 때문에   
.shutdown()메서드를 통해 종료시켜야 한다.
```java
ExecutorService executorSingleService = Executors.newSingleThreadExecutor();
executorSingleService.execute(() -> System.out.println("Thread : " + Thread.currentThread().getName()));
//.shutdown() 메서드를 통해 종료시켜줘야한다.
executorSingleService.shutdown();
```
---
### Thread 단건 실행 방법
- `newSingleThreadExecutor();`
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();
// Thread를 생성할 때와 마찬가지로 Runnable()을 이용한다.
executorService.execute(new Runnable() {
    @Override
    public void run() {
        System.out.println("Thread : " + Thread.currentThread().getName());
    }
});

//람다식으로 변경 가능
executorService.execute(() -> System.out.println("Thread : " + Thread.currentThread().getName()));
```
---

### Thread 여러개 실행 방법
- `newFixedThreadPool(int n);`
- Thread Pool안에 n개의 Thread를 만들어서 task들을 처리한다.

대기중인 task들은 BlockingQueue에 쌓여서 순서를 기다린다.
```java
//Thread Pool 생성
ExecutorService executorThreadPoolService = Executors.newFixedThreadPool(2);
executorThreadPoolService.execute(() -> System.out.println("Thread1 : " + Thread.currentThread().getName()));
executorThreadPoolService.execute(() -> System.out.println("Thread2 : " + Thread.currentThread().getName()));
executorThreadPoolService.execute(() -> System.out.println("Thread3 : " + Thread.currentThread().getName()));
executorThreadPoolService.execute(() -> System.out.println("Thread4 : " + Thread.currentThread().getName()));
executorThreadPoolService.execute(() -> System.out.println("Thread5 : " + Thread.currentThread().getName()));

executorThreadPoolService.shutdown();
```
---

### Thread 반복 실행 방법
- `newSingleThreadScheduledExecutor();`
- 스케쥴을 이용해 반복적으로 실행 시킬 수 있다.

```java
//스케쥴을 이용해 반복적으로 실행 시킬 수 있다.
ScheduledExecutorService executorScheduledService = Executors.newSingleThreadScheduledExecutor();
executorScheduledService.scheduleAtFixedRate(() -> System.out.println("Thread : " + Thread.currentThread().getName()), 1, 2, TimeUnit.SECONDS);
```
---


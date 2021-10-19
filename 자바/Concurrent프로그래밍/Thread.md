# Concurrent 프로그래밍

---
- Concurrent 소프트웨어는 동시에 여러 작업을 할 수 있는 소프트웨어를 뜻한다.   
가령 브라우저로 쇼핑을 하면서 노래를 듣는다던가, 너튜브를 보면서 Intellij로 코딩을 한다던가.   
  

- 자바진형에서는 Concurrent 프로그래밍을 멀티프로세싱, 멀티쓰레드 두 가지 모두 지원한다.
---
### 메인 Thread에서 Sub Thread를 만들수 있는 방법

1. Thread를 extends 받는 방법 (불ㅡㅡㅡㅡ편)
```java

    public static void main(Stringp[] args){
        MyThread myThread = new MyThread();
        myThread.start();
    }

    static class MyThread extends Thread {
        @Override
        public void run() {
            System.out.println("Thread : " + Thread.currentThread().getName());
        }
    }
```

2. Runnable()을 구현하는 방법
```java
public static void main(Stringp[] args){
    
    Thread thread = new Thread(new Runnable(){
        @Override
        public void run(){
            System.out.println("Thread:"+Thread.currentThread().getName());
        }
    });
    
}

    - 람다식으로 간단하게 바꿔서 사용할 수도 있다.
    Thread thread = new Thread(() -> System.out.println("Thread:" + Thread.currentThread().getName()));
```
---
## Thread 주요 기능

### .sleep()
- 현재 Thread를 재우는것 ( 대기시키는 것 )

현재 Thread는 대기를 하고 있으니 다른 Thread가 먼저 실행된다.
```java
public static void main(Stringp[] args){
    
    Thread thread = new Thread(() -> {
        try {
            //sleep -> 쓰레드를 대기시킨다.
            Thread.sleep(1000L);
        } catch (InterruptedException e) {
            throw new IllegalStateException(e);
        }
        //이 Thread는 1초간 대기를 하니까 Main Thread가 먼저 실행된 후 Sub Thread가 실행된다.
        System.out.println("Sub : " + Thread.currentThread().getName());
    });
    thread.start();
    
    //이 Main Thread가 먼저 출력되고, 위의 Sub Thread가 출력된다.
    System.out.println("Main : " + Thread.currentThread().getName());
}
```
---
### .interrupt()
- .interrupt()를 통해 Thread를 종료 시킬수 있다.

.interrupt() 자체로 Thread를 종료시키는 메서드는 아니다.   
단순히 대기중(.sleep())인 Thread에 InterruptedException을 발생시켜 Thread를 깨우는 것이다.
```java
public static void main(Stringp[] args){
    
    Thread thread = new Thread(() -> {
        //무한 루프를 만든다.
        while(true){
            try {
                System.out.println("Sub : " + Thread.currentThread().getName());
                Thread.sleep(1000L);
            } catch (InterruptedException e) {
                //return 값을 void로 주면 Thread가 종료되는걸 이용한다.
                //.interrupt()를 사용해 Thread를 종료 시킨다.
                return;
            }
        }
        
    });
    thread.start();
    Thread.sleep(3000L);
    
    //쓰레드에 InterruptedException를 발생시킨다.
    thread.interrupt();
}
```
---
### .join()
- .join()은 다른 Thread를 기다리는 것이다.
```java
public static void main(Stringp[] args){
    
    Thread thread = new Thread(() -> {
        try {
            System.out.println("Sub : " + Thread.currentThread().getName());
            Thread.sleep(5000L);
        } catch (InterruptedException e) {
            throw new IllegalStateException(e);
        }
        
    });
    thread.start();
    
    //.join()을 하게되면 위의 5초간 sleep시킨 Sub Thread가 끝날때까지 기다린다.
    thread.join();
    //그리고 이 Main Thread가 출력된다.
    System.out.println("Main : " + Thread.currentThread().getName());
}
```
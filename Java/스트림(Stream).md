# 스트림( Stream )

---
- 스트림은 데이터를 담고 있는 저장소(컬렉션)이 아니다.
- Stream이 처리하는 데이터의 원본 소스를 변경하지 않는다.
- Stream으로 처리하는 데이터는 딱 한번만 처리한다.
- 중개 오퍼레이션은 근본적으로 lazy하다.
  - 종료형 오퍼레이션이 오기 전까지 중개형 오퍼레이션은 실행되지 않는다.
    - 중개 오퍼레이션 - filter, map, limit, skip, sorted .... ( 스트림을 리턴 O)
    - 종료 오퍼레이션 - collect, allMatch, count, forEach, min, max .... (스트림 리턴 X)
- 스트림 파이프라인에는 0개 혹은 여러개의 중개오퍼레이션이 올 수 있고, 단 하나의 종료 오퍼레이션이 존재한다.
---
.map()으로 끝나면 map()은 중개형 오퍼레이션이기 때문에 실행되지 않는다.
```java
names.stream().map((s) -> {
    System.out.println(s);
    return s.toUpperCase();
});
```
<br>

.collect()는 종료형 오퍼레이터이기 때문에 중개형 오퍼레이터인 .map()이 실행이 된다.
```java
List<String> collect = names.stream().map((s) -> {
    System.out.println(s);
    return s.toUpperCase();
}).collect(Collectors.toList());
```
<br>

stream을 쓰다보면 그냥 단순 for문으로도 처리할 수 있을거같은데 굳이 stream❓   
이유는 루프를 돌면서 엘리먼트들을 처리하기가 어렵기 때문이다. stream을 사용하면 병렬처리가 쉽게 해결된다❗

parallelStream()을 사용하면 JVM이 알아서 병렬적으로 처리해준다. 하지만 무조건 병렬처리로 사용하는것이 좋은것은 아니다.   
parallelStream()은 여러 스레드에 나눠서 처리한다. 그냥 stream()은 하나의 메인 스레드에서 처리가된다.   
```java
List<String> collect1 = names.parallelStream().map(String::toUpperCase)
.collect(Collectors.toList());
collect1.forEach(System.out::println);
```

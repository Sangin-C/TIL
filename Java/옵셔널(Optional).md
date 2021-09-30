# 옵셔널( Optional )


기존에는 null 체크를 이런 식으로 해왔다.   
개발자가 일일이 이렇게 체크를해줘야하는데 당연히 까먹을수 있고 실수할 수 있다.   
그래서 Optional이라는것이 나왔다.
```java
Test test = spring_boot.getTest();
if (test != null){
    System.out.println(test.getTest());
}
```
---

### .isPresent()
.isPresent()는 값이 있는지 체크한다.   
- 존재하면 true, 없으면 false
```java
Optional<Test> optional = test.stream()
        .filter(oc -> oc.getTitle().startsWith("spring"))
        .findFirst();

boolean present = optional.isPresent();
System.out.println(present);
```
---

### .isEmpty()
.isEmpty()는 값이 비어있는지 확인한다. (JAVA11 부터 제공)   
- 비어있으면 true, 존재하면 false
```java
Optional<Test> optional = test.stream()
        .filter(oc -> oc.getTitle().startsWith("spring"))
        .findFirst();

boolean empty = optional.isEmpty();
System.out.println(empty);
```
---

### .get()
.get()을 하면 Optional에 있는 값을 꺼내올 수 있다.   
- 값이 있으면 문제가 없지만, 값이 없는걸 꺼내려고하면 Runtime Exception이 터진다.   
- Optional값이 있는지 확인하고 써야한다.   
- if문보다는 Optional이 제공해주는 API를 사용하는게 좋다.   
```java
Optional<Test> optional = test.stream()
        .filter(oc -> oc.getTitle().startsWith("spring"))
        .findFirst();

if (optional.isPresent()){
    OnlineClass onlineClass=optional.get();
    System.out.println(onlineClass.getTitle());
}
```
---

### .ifPresent()
.ifPresent()는 값이 있는 경우에 그 값을 처리(가공)할 수 있다.   
```java
Optional<Test> optional = test.stream()
        .filter(oc -> oc.getTitle().startsWith("spring"))
        .findFirst();
optional.ifPresent(oc -> System.out.println(oc.getTitle()));
```
---

### .orElse()
.orElse()는 값이 있으면 가져오고 없으면 어떤 행위를 할 수있다.   
.orElse()는 값이 있어도 파라미터를 실행하고, 없어도 파라미터를 실행한다.
```java
Optional<Test> optional = test.stream()
        .filter(oc -> oc.getTitle().startsWith("spring"))
        .findFirst();
Test test = optional.orElse(createNewJpaClass());
System.out.println(onlineClass.getTitle());
```
---

### .orElseGet()
.orElseGet()은 값이 있으면 가져오고 없으면 어떤 행위를 할 수있다.   
.orElseGet()은 값이 없을때만 파라미터를 실행한다. (or.Else와의 차이점)
```java
Optional<Test> optional = test.stream()
        .filter(oc -> oc.getTitle().startsWith("spring"))
        .findFirst();
        
Test testGet = optional.orElseGet(() ->createTest());
System.out.println(testGet.getTitle());
```
---

### .orElseThrow()
.orElseThrow()은 Optinal에 값이 있으면 가져오고 값이 없으면 Exception을 던진다.   
- 원하는 에러가 있으면 작성하면된다.
```java
Optional<Test> optional = test.stream()
        .filter(oc -> oc.getTitle().startsWith("spring"))
        .findFirst();

Test testThrow = optional.orElseThrow(() -> {
    return new IllegalArgumentException();
});
System.out.println(testThrow.getTitle());
```
---

### .filter()
.filter()는 값이 있다는 가정하에 쓰는것이다.   
- 값이 없으면 아무것도 리턴해주지 않는다.   
- Optinal에 들어있는 값을 걸러낼때 쓴다.
```java
Optional<Test> testFilter = optional.filter(oc -> !oc.isClosed());
System.out.println(testFilter.isEmpty());
```
---

### .map()
.map()은 Optional에 있는 값을 변환할때 쓰인다.
```java
Optional<Integer> integer = optional.map(oc -> {
    return oc.getId();
});
```
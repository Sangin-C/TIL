# Parallel Sort
- Fork/Join 프레임워크를 사용해서 배열을 병렬로 정렬하는 기능을 제공한다.
- 동작방식은 배열을 둘로 계속해서 쪼갠 후 어느정도 쪼개어졌으면 합치면서 정렬을 한다.
---

### Serial Sort방식
- 하나의 Thread만을 이용해서 Sorting한다.
```java
int size = 1500;
int[] numbers = new int[size];
Random random = new Random();
IntStream.range(0, size).forEach(i -> numbers[i] = random.nextInt());

long start = System.nanoTime();
Arrays.sort(numbers);
System.out.println("serial sorting took " + (System.nanoTime() - start));
```
---

### Parallel Sort 방식
- 여러개의 Thread를 이용해서 Sort하기 때문에 정렬해야할 값이 많다면 Parallel Sort방식이 빠르다.
```java
IntStream.range(0, size).forEach(i -> numbers[i] = random.nextInt());
start = System.nanoTime();
Arrays.parallelSort(numbers);
System.out.println("parallel sorting took " + (System.nanoTime() - start));
```
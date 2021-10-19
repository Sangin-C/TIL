# PermGen vs Metaspace
- JAVA8에 오면서 기존의 JVM영역에 속해 있던 PermGen 메모리 영역이 없어지고, Metaspace 영역으로 변경 되었다.
---

## 1. PermGen
- Permanet generation의 약자로 클래스의 메타데이터를 담고 있는 영역이다.
- PermGen영역은 Heap메모리에 속해있다.
- 기본값으로 제한된 크기를 가지고 있기 때문에 GC에의해 정리가 되더라도 너무 많아지면 Out Of Memory Exception이 발생한다.
---
## 2. Metaspace
- PermGen과 마찬가지로 클래스 메타데이터를 담고 있는 영역이다.
- PermGen과는 달리 Heap메모리 영역이 아니라, Native 메모리 영역에 속해있다.   
  (Native 메모리 -> OS 메모리)
  
- PermGen과는 달리 기본값으로 제한된 크기를 가지고 있지 않고, 계속해서 늘어난다.
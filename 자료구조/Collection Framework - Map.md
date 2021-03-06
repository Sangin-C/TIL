# Collection Framework - Map
- 컬렉션 프레임워크(Collection Framework)는 다수의 데이터를 쉽고 효과적으로 처리할 수 있는 표준화된 방법을 제공하는 클래스의 집합을 의미한다.
- 즉, 데이터를 저장하는 자료 구조와 데이터를 처리하는 알고리즘을 구조화화여 클래스로 정의해 놓은 것이다.
## Set Interface
- Map 인터페이스는 Collection 인터페이스를 상속받지 않고 별도로 정의된다.
### 특징
1. 키(key)와 값(value)로 구성된 데이터를 저정하는 구조를 가지고 있다. 
2. 키는 중복을 허용하지 않지만, 값은 중복을 허용한다. 중복된 키 값이 들어오면 새로운 값으로 대체된다.
3. 순서가 유지되지 않는 집합이다.

---
## HashMap
### 특징
- Map 인터페이스를 구현한 클래스로써 키와 값으로 구성된 Entry객체를 저장하는 구조를 가지고 있다.
- 값은 중복 저장될 수 있지만 키는 중복 저장될 수 없습니다.
### 장점
- HashMap은 이름 그대로 해싱(Hashing)을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보인다.
### 단점
- 자동 정렬을 해주지 않는다.

---
## TreeMap
### 특징
- Map 인터페이스를 구현한 클래스로써 키와 값으로 구성된 Entry객체를 저장하는 구조를 가지고 있다.
- 이진 탐색 트리(BinarySearchTree)로 구성되어있다. 이진 탐색 트리 중에서도 `레드-블랙 트리`로 구성되어있다.
### 장점
- 자동 정렬을 해준다.
- 검색과 정렬에 유리하다.
- 정렬된 상태로 Map을 유지해야 하거나 정렬된 데이터를 조회해야 하는 범위 검색이 필요한 경우 TreeMap을 사용하는 것이 효율성면에서 유리하다.
### 단점
- 데이터를 저장할때 즉시 정렬하기 때문에 HashMap보다 데이터의 삽입/삭제 시간은 더 오래 걸린다.

---
## 정리
### HashMap
`삽입 삭제가 자주 발생하며, 검색/정렬이 빈번하지 않을경우 유리하다.`
### TreeMap
`삽입 삭제가 자주 발생하지 않으며, 검색/정렬이 빈번할 경우 유리하다.`


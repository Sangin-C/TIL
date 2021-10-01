# 데이트(Date)


기존에는 이렇게 날짜 형식을 받아와서 사용했다.   
Date에서 날짜도 받아오고 시간도 받아올 수 있다. API가 명확하지 않았다.
```java
Date date = new Date();
long time = date.getTime();
System.out.println(date);
System.out.println(time);
```
---

## 기계용 날짜 API


### Instant
- 기계용 API는 어떤 시간을 재거나 메소드 실행시간을 비교하거나 할때 쓰인다.   
```java
Instant instant = Instant.now();
System.out.println(instant);    //기준시 UTC, GMT
```
---

### ZoneId
- 현재 내가 있는 지역 나타내기
```java
ZoneId zone = ZoneId.systemDefault();
System.out.println(zone);
```
---

### ZonedDateTime
- 현재 내가 있는 지역의 시간 나타내기
```java
ZonedDateTime zonedDateTime = instant.atZone(ZoneId.systemDefault());
System.out.println(zonedDateTime);
```
---

### Duration
- 시간 비교하기
```java
Duration between = Duration.between(now, plus);
System.out.println(between.getSeconds());
```
---

## 사람용 API

### LocalDateTime
- 현재 시간
```java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println(localDateTime);
```
- 특정 시간
```java
LocalDateTime birthday = LocalDateTime.of(1993, Month.SEPTEMBER, 9, 3, 0, 30);
System.out.println(birthday);
```
---

### ZonedDateTime
- 특정 존의 시간 나타내기
```java
ZonedDateTime nowInKorea = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
System.out.println(nowInKorea);
```
---

### LocalDate
- 기간 나타내기
```java
LocalDate today = LocalDate.now();
LocalDate thisYearBirthday = LocalDate.of(2021, Month.JULY, 15);
```
---

### Period
- 기간 비교하기(between)
```java
Period period = Period.between(today, thisYearBirthday);
System.out.println(period.getDays());
```
- 기간 비교하기(until)
```java
Period until = today.until(thisYearBirthday);
System.out.println(until.getDays());
```
---

### DateTimeFormatter
- 날짜 포맷팅
```java
DateTimeFormatter MMddyyyy = DateTimeFormatter.ofPattern("MM/dd/yyyy");
System.out.println(localDateTime.format(MMddyyyy));
```
- 날짜 파싱
```java
LocalDate parse = LocalDate.parse("07/15/1982", MMddyyyy);
System.out.println(parse);
```

# DateTime

```java
DateTime dateTime = DateTime.now().withTimeAtStartOfDay();
System.out.println(dateTime);
//2024-07-12T00:00:00.000+08:00 
```

`DateTime.now().withTimeAtStartOfDay()`返回一天的开始

# 增加/减少时间

```java
DateTime dateTime = DateTime.now();

DateTime oneWeekLater = dateTime.plusWeeks(1);
DateTime threeDaysBefore = dateTime.minusDays(3);
```

# 时间差值

```java
DateTime start = new DateTime(2024, 7, 10, 12, 0);
DateTime end = new DateTime(2024, 7, 12, 14, 30);

int daysBetween = Days.daysBetween(start, end).getDays();
int hoursBetween = Hours.hoursBetween(start, end).getHours();
int minutesBetween = Minutes.minutesBetween(start, end).getMinutes();
int secondsBetween = Seconds.secondsBetween(start, end).getSeconds();
```


---
title: Date and Time
tags:
  - JavaSE
related_topics:
  - "[[Dmdev Date and Time]]"
created: 2024-08-31 13:11
modified: 2024-09-12T13:11:03+03:00
difficulty: medium
questions: 
notes: 
links: 
---
# Date and Time

Две крутые статьи:

[https://habr.com/ru/articles/274811/](https://habr.com/ru/articles/274811/)

[https://habr.com/ru/articles/274905/](https://habr.com/ru/articles/274905/)

### Общая информация

<mark class="hltr-red">GMT(Greenwich mean time)</mark> - <mark class="hltr-yellow">астрономическое время, секунда не точная, зависит от колебаний во вращенни Земли.</mark>

<mark class="hltr-red">UTC(Coordinated universal time) </mark>- <mark class="hltr-yellow">атомное время, секунда точная, но их приходится подводить с помощью leap second</mark>

<mark class="hltr-red">Unix-Time</mark> - Unix время, <mark class="hltr-yellow">секунда точная, но часы не подводятся, leap second проблема, она решается синхронизацией по NTP.</mark> Система описания моментов во времени, определяется как количество миллисекунд, прошедших с полуночи (00:00:00 UTC) 1 января 1970 года

Unix-Time-JVM - Unix-Time*1000(т.к с миллисекундами)

TimeStamp SQL - Unix-Time*1000_000_000(т.к с наносекундами)

Все новые классы Java8 -Unix-Time*1000_000_000(т.к с наносекундами)

12 p.m полдень

12 a.m полночь

## packaje java.util устаревшие классы

### Class Date

- java.util.Date
    
- All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)<[Date](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html)>
    
    Direct Known Subclasses:[Date](https://docs.oracle.com/javase/8/docs/api/java/sql/Date.html), [Time](https://docs.oracle.com/javase/8/docs/api/java/sql/Time.html), [Timestamp](https://docs.oracle.com/javase/8/docs/api/java/sql/Timestamp.html)
    
    **Большинство методов класса DATE deprecated.**
    
    Класс `Date` хранит информацию о дате и времени в виде количества миллисекунд, прошедших с 1 января 1970 года. Для их хранения используется тип `long`.
    
    Чтобы получить текущее время и дату, достаточно просто создать объект типа `Date`. Каждый новый объект хранит время (момент) его создания.
    
    ```java
    Date current = new Date();
    ```
    
    Cоздать объект `Date`, который бы содержал другую дату или время:
    
    ```java
    Date birthday = new Date(год, месяц, день);
    ```
    
    Есть два нюанса:
    
    1. Год нужно задавать от 1900.
    2. Месяцы нумеруются с нуля.
    
    ```java
    Date current = new Date(89, 2, 21);
    System.out.println(current); //Tue Mar 21 00:00:00 EET 1989
    ```
    
    Чтобы получить количество миллисекунд, прошедшее с 1 января 1970 года:
    
    ```java
    long time = date.getTime(); // deprecated
    ```
    
    Метод `getTime()` возвращает количество миллисекунд, которое хранится внутри объекта `Date`.
    
    Можно изменить количество миллисекунд в существующем объекте:
    
    ```java
    Date date = new Date();
    date.setTime(1117876500000L);
    
    // или
    
    Date date = new Date(1117876500000L);
    ```
    
    Чтобы сравнивать объекты класса Date существуют методы `before` и `after`
    
    ```java
    if (date1.before(date2))
    
    if (date2.after(date1))
    ```
    
    Класс `Date` умеет делать еще одну интересную и полезную вещь: . Или как говорят программисты, парсить строку.
    
    Чтобы получить дату из строки - используется метод — `parse()` (deprecated). Выглядит процесс парсинга так:
    
    ```java
    Date date = new Date();
    date.setTime( Date.parse("Jul 06 12:15:00 2019") );
    ```
    
    Чтобы отформатировать Date можно использовать класс SimpleDateFormatter наследник DateFormatter
    
    [https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html)
    
    ```java
    Date current = new Date(105, 5, 4, 12, 15, 0);
    SimpleDateFormat formatter = new SimpleDateFormat("MMM-dd-YYYY");
    String message = formatter.format(current);
    System.out.println(message);
    ```
    

### class Calendar

[https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html)

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)<[Calendar](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html)>

Direct Known Subclasses:[GregorianCalendar](https://docs.oracle.com/javase/8/docs/api/java/util/GregorianCalendar.html)

Программисты хотели иметь «умный класс `Date`». И такой класс появился: им стал класс `Calendar`. Он задумывался как инструмент не только хранения, но и сложной работы с датами.

Статический метод `getInstance()` класса `Calendar` создаст объект `Calendar`, инициализированный текущей датой. В зависимости от настроек компьютера, на котором выполняется программа, будет создан нужный календарь. :

```java
Calendar date = Calendar.getInstance();
```

У класса Calendar есть наследник GregorianCalendar - самый распространенный в мире.

Объект календарь с произвольной датой создается командой:

```java
Calendar date = new GregorianCalendar(год, месяц, день);
```

Чтобы задать не только дату, но и время, нужно передать их дополнительными параметрами:

```java
... = new GregorianCalendar(год, месяц, день, часы, минуты, секунды);
```

Чтобы получить фрагмент даты (год, месяц, ...), у класса `Calendar` есть специальный метод — `get()`

```java
Calendar calendar = Calendar.getInstance();

int era = calendar.get(Calendar.ERA);
int year = calendar.get(Calendar.YEAR);
int month = calendar.get(Calendar.MONTH);
int day = calendar.get(Calendar.DAY_OF_MONTH);

int dayOfWeek = calendar.get(Calendar.DAY_OF_WEEK);
int hour = calendar.get(Calendar.HOUR);
int minute = calendar.get(Calendar.MINUTE);
int second = calendar.get(Calendar.SECOND);
```

Для изменения фрагмента даты (год, месяц, …), у класса Calendar есть специальный метод - `set()`

```java
Calendar calendar = new GregorianCalendar();

calendar.set(Calendar.YEAR, 2019);
calendar.set(Calendar.MONTH, 6);
calendar.set(Calendar.DAY_OF_MONTH, 4);
calendar.set(Calendar.HOUR_OF_DAY, 12);
calendar.set(Calendar.MINUTE, 15);
calendar.set(Calendar.SECOND, 0);

System.out.println(calendar.getTime());
```

В классе `Calendar` есть константы не только для названия фрагментов даты:

```java
Calendar date = new GregorianCalendar(2019, Calendar.JANUARY, 31);
if (calendar.get(Calendar.DAY_OF_WEEK) == Calendar.FRIDAY)
{
   System.out.println("Это пятница");
}
```

Чтобы проводить операции по добавлению к дате годов, месяцев и т.д существует метод `add().`

```java
Calendar calendar = new GregorianCalendar(2019, Calendar.FEBRUARY, 27);
calendar.add(Calendar.DAY_OF_MONTH, 2);
System.out.println(calendar.getTime());
```

Чтобы прокрутить дату существует метод `roll()`

```java
Calendar calendar = new GregorianCalendar(2019, Calendar.FEBRUARY, 27);
calendar.roll(Calendar.MONTH, -2);
System.out.println(calendar.getTime()); //Fri Dec 27 00:00:00 EET 2019
```

Также можно устанавливать TimeZone

```java
Calendar calendar = Calendar.getInstance();
        calendar.setTimeZone(TimeZone.getTimeZone("Asia/Shanghai"));
        calendar.set(1970, 0,1,0,0);
```

## DateTime Api

Пакет `java.time` — базовый пакет для Java Date Time API: в нем содержатся такие классы как `LocalDate`, `LocalTime`, `LocalDateTime`, `Instant`, `Period`, `Duration`. <mark class="hltr-red">Все объекты этих классов</mark> — `immutable`: <mark class="hltr-yellow">их нельзя изменить после создания.</mark>

Пакет `java.time.format` содержит в себе<mark class="hltr-yellow"> классы для форматирования времени</mark>: преобразования времени (и даты) в текстовую строку и обратно. Например, в нем содержится такой<mark class="hltr-yellow"> универсальный класс</mark> как `DateTimeFormatter`, который пришел на смену `SimpleDateFormat`.

Пакет `java.time.zone` <mark class="hltr-yellow">содержит классы для работы с часовыми поясами</mark> (time zones). Он содержит такие классы как `TimeZone` и `ZonedDateTime`. Если вы пишете код для сервера, клиенты которого находятся в разных частях света, эти классы вам очень понадобятся.

### Документация, общий список классов

[https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html](https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html)

<mark class="hltr-red">Все объекты этих классов </mark>— `immutable`: <mark class="hltr-red">их нельзя изменить после создания.</mark>

| Name                                                                                      | Description                                                                                                         |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [Clock](https://docs.oracle.com/javase/8/docs/api/java/time/Clock.html)                   | A clock providing access to the current instant, date and time using a time-zone.                                   |
| [Duration](https://docs.oracle.com/javase/8/docs/api/java/time/Duration.html)             | A time-based amount of time, such as '34.5 seconds'.                                                                |
| [Instant](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html)               | An instantaneous point on the time-line.                                                                            |
| [LocalDate](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html)           | A date without a time-zone in the ISO-8601 calendar system, such as `2007-12-03`.                                   |
| [LocalDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)   | A date-time without a time-zone in the ISO-8601 calendar system, such as `2007-12-03T10:15:30`.                     |
| [LocalTime](https://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html)           | A time without a time-zone in the ISO-8601 calendar system, such as `10:15:30`.                                     |
| [MonthDay](https://docs.oracle.com/javase/8/docs/api/java/time/MonthDay.html)             | A month-day in the ISO-8601 calendar system, such as `--12-03`.                                                     |
| [OffsetDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/OffsetDateTime.html) | A date-time with an offset from UTC/Greenwich in the ISO-8601 calendar system, such as `2007-12-03T10:15:30+01:00`. |
| [OffsetTime](https://docs.oracle.com/javase/8/docs/api/java/time/OffsetTime.html)         | A time with an offset from UTC/Greenwich in the ISO-8601 calendar system, such as `10:15:30+01:00`.                 |
| [Period](https://docs.oracle.com/javase/8/docs/api/java/time/Period.html)                 | A date-based amount of time in the ISO-8601 calendar system, such as '2 years, 3 months and 4 days'.                |
| [Year](https://docs.oracle.com/javase/8/docs/api/java/time/Year.html)                     | A year in the ISO-8601 calendar system, such as `2007`.                                                             |
| [YearMonth](https://docs.oracle.com/javase/8/docs/api/java/time/YearMonth.html)           | A year-month in the ISO-8601 calendar system, such as `2007-12`.                                                    |
| [ZonedDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html)   | A date-time with a time-zone in the ISO-8601 calendar system, such as `2007-12-03T10:15:30+01:00 Europe/Paris`.     |
| [ZoneId](https://docs.oracle.com/javase/8/docs/api/java/time/ZoneId.html)                 | A time-zone ID, such as `Europe/Paris`.                                                                             |
| [ZoneOffset](https://docs.oracle.com/javase/8/docs/api/java/time/ZoneOffset.html)         | A time-zone offset from Greenwich/UTC, such as `+02:00`.                                                            |

### class LocalDate

[https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html)

- Чтобы получить текущую дату, нужно воспользоваться статическим методом `now()`. Это гораздо проще, чем кажется:

```java
LocalDate today = LocalDate.now();
```

- У класса `LocalDate` есть разновидность метода `now(ZoneId)`, который <mark class="hltr-yellow">позволяет получить текущую дату в определенном часовом поясе.</mark>

Для этого нам понадобится еще один класс — `ZoneId` (java.time.ZoneId). У него есть метод `of()`, который возвращает объект `ZoneId` по имени часового пояса.

```java
ZoneId  timezone = ZoneId.of("Asia/Shanghai");
LocalDate today = LocalDate.now(timezone);
System.out.println("Сейчас в Шанхае = " + today);
```

- Чтобы получить объект `LocalDate`, содержащий определенную дату, нужно воспользоваться статическим методом `of()`.

```java
LocalDate date = LocalDate.of(2019, Month.FEBRUARY, 22);
```

```java
LocalDate today = LocalDate.of(2019, 2, 22); //нумерация с 1
System.out.println("Сегодня = " + today); //2019-02-22
```

- Чтобы получить дату, имея только номер года и номер дня года существует метод `ofYearDay()`

```java
LocalDate date = LocalDate.ofYearDay(год, день);
LocalDate today = LocalDate.ofYearDay(2019, 100); //Сегодня = 2019-04-10
```

- Существует метод `ofEpochDay()`,который возвращает дату, отсчитанную от 1 января 1970 года.

```java
LocalDate localDate = LocalDate.ofEpochDay(10000);
        System.out.println(localDate); //1997-05-19
```

- Получение даты в определенном часовом поясе с помощью `now(ZoneId).`

Для этого нам понадобится еще один класс — `ZoneId` (java.time.ZoneId). У него есть метод `of()`, который возвращает объект `ZoneId` по имени часового пояса.

`ZoneId` [https://docs.oracle.com/javase/8/docs/api/java/time/ZoneId.html](https://docs.oracle.com/javase/8/docs/api/java/time/ZoneId.html)

```java
ZoneId  timezone = ZoneId.of("Asia/Shanghai");
LocalDate today = LocalDate.now(timezone);
System.out.println("Сейчас в Шанхае = " + today);
```

- Получение фрагментов даты:

![[Untitled (1).png]]

- Класс `LocalDate` содержит несколько методов, которые позволяют работать с датой. <mark class="hltr-yellow">Эти методы реализованы по аналогии с методами класса</mark> `String`: <mark class="hltr-purple">каждый из этих методов не меняет существующий объект LocalDate, а возвращает новый с нужными данными.</mark>

|`plusDays(int days)`|Добавляет определенное количество дней к дате|
|---|---|
|`plusWeeks(int weeks)`|Добавляет недели к дате|
|`plusMonths(int months)`|Добавляет месяцы к дате|
|`plusYears(int years)`|Добавляет годы к дате|
|`minusDays(int days)`|Отнимает дни от даты|
|`minusWeeks(int weeks)`|Отнимает недели от даты|
|`minusMonths(int months)`|Отнимает месяцы от даты|
|`minusYears(int years)`|Отнимает годы от даты|

### class LocalTime

[https://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html](https://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html)

Класс `LocalTime` создан для случаев, когда нужно работать только со временем без даты.

- Чтобы создать новый объект класса `LocalTime`, нужно воспользоваться статическим методом `now()`. Пример:

```java
LocalTime time = LocalTime.now();
```

- Чтобы получить заданное время, нужно воспользоваться статическим методом `of()`.

```java
LocalTime time = LocalTime.of(часы, минуты, секунды, наносекунды);
LocalTime time = LocalTime.of(часы, минуты, секунды);
LocalTime time = LocalTime.of(часы, минуты);

LocalTime time = LocalTime.of(12, 15, 0, 100);
System.out.println("Сейчас = " + time); //Сейчас = 12:15:00.000000100
```

- Также можно получить время по номеру секунды в сутках: для этого есть специальный статический метод `ofSecondOfDay()`:

```java
LocalTime time = LocalTime.ofSecondOfDay(секунды);

LocalTime time = LocalTime.ofSecondOfDay(10000);
System.out.println(time); // 02:46:40
```

- Чтобы из объекта `LocalTime` получить значение определенного элемента времени, используют специальные методы:

|Метод|Описание|
|---|---|
|`int getHour()`|Возвращает часы|
|`int getMinute()`|Возвращает минуты|
|`int getSecond()`|Возвращает секунды|
|`int getNano()`|Возвращает наносекунды|

```java
LocalTime now = LocalTime.now();
System.out.println(now.getHour());
System.out.println(now.getMinute());
System.out.println(now.getSecond());
System.out.println(now.getNano());
```

- Класс `LocalTime` также содержит методы, которые позволяют работать со временем. Эти методы реализованы по аналогии с методами класса `LocalDate`: каждый из них не меняет существующий объект `LocalTime`, а возвращает новый с нужными данными.

|Метод|Описание|
|---|---|
|`plusHours(int hours)`|Добавляет часы|
|`plusMinutes(int minutes)`|Добавляет минуты|
|`plusSeconds(int seconds)`|Добавляет секунды|
|`plusNanos(int nanos)`|Добавляет наносекунды|
|`minusHours(int hours)`|Вычитает часы|
|`minusMinutes(int minutes)`|Вычитает минуты|
|`minusSeconds(int seconds)`|Вычитает секунды|
|`minusNanos(int nanos)`|Вычитает наносекунды|

```java
LocalTime time = LocalTime.now();
LocalTime time2 = time.plusHours(2);
LocalTime time3 = time.minusMinutes(40);
LocalTime time4 = time.plusSeconds(3600);
```

### class LocalDateTime

[https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html)

Класс `LocalDateTime` объединяет в себе возможности классов `LocalDate` и `LocalTime`: он хранит и дату, и время. Его объекты тоже неизменяемые, и его методы также аналогичны методам классов `LocalDate` и `LocalTime`.

- Получение определенного момента: дата и время

Сначала идут параметры, которые задают дату в тех же форматах, что и в классе `LocalDate`. Затем идут параметры, задающие время в тех же форматах, что и в классе `LocalTime`

```java
of(int year, Month month, int day, int hour, int minute, int second, int nano)
LocalDateTime date = LocalDateTime.of(2019, Month.MAY, 15, 12, 15, 00);
```

- Также можно создать LocalDateTime передав в конструктор `LocalDate` и `LocalDateTime`

```java
LocalDate date = LocalDate.now();
LocalTime time = LocalTime.now();
LocalDateTime current = LocalDateTime.of(date, time);
```

### class Instant

<mark class="hltr-red">Пришел на замену Date</mark>. <mark class="hltr-yellow">Хранит в себе количество секунд, прошедших с 1 января 1970 года и количество наносекунд.</mark>

- Получить объект `Instant` можно точно так же, как объект `LocalTime`:

```java
Instant timestamp = Instant.now();
```

- Такж<mark class="hltr-yellow">е можно создать новый объект с помощью разновидностей метода</mark> `of()`, если передать в него время, прошедшее с 1 января 1970 года:

|`ofEpochMilli(long milliseconds)`|Нужно передать количество миллисекунд|
|---|---|
|`ofEpochSecond(long seconds)`|Нужно передать количество секунд|
|`ofEpochSecond(long seconds, long nanos)`|Нужно передать секунды и наносекунды|

- Методы, которые возвращают хранящиеся значения:

|`long getEpochSecond()`|Количество секунд, прошедшее с 1 января 1970 года|
|---|---|
|`int getNano()`|Наносекунды.|
|`long toEpochMilli()`|Количество миллисекунд, прошедшее с 1 января 1970 года|

- Также есть методы, которые способны создавать новые объекты `Instant` на основе уже существующего:

|`Instant plusSeconds(long)`|Добавляет секунды к текущему моменту времени|
|---|---|
|`Instant plusMillis(long)`|Добавляет миллисекунды|
|`Instant plusNanos(long)`|Добавляет наносекунды|
|`Instant minusSeconds(long)`|Вычитает секунды|
|`Instant minusMillis(long)`|Вычитает миллисекунды|
|`Instant minusNanos(long)`|Вычитает наносекунды|

### class ZonedDateTime

[https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html](https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html)

Для хранения временной зоны в Java используется класс `ZoneId` из пакета `java.time`.

- Чтобы получить список всех зон, нужно использовать `getAvailableZoneIds()`

```java
for (String s: ZoneId.getAvailableZoneIds())
   System.out.println(s);
```

- При создании объекта `ZonedDateTime` нужно вызвать у него статический метод `now()` и передать в него объект `ZoneId`. Если в метод `now()` <mark class="hltr-yellow">не передать объект</mark> `ZoneId`, а так можно, <mark class="hltr-yellow">временная зона будет определена автоматически: на основе настроек компьютера, на котором выполняется программа.</mark>

```java
ZoneId zone = ZoneId.of("Africa/Cairo");
ZonedDateTime time = ZonedDateTime.now(zone);
```

- Одной из интересных особенностей `ZonedDateTime`<mark class="hltr-green2"> является возможность его преобразования в локальную дату и время. Пример.</mark>

```java
ZoneId zone = ZoneId.of("Africa/Cairo");
ZonedDateTime cairoTime = ZonedDateTime.now(zone);

LocalDate localDate = cairoTime.toLocalDate();
LocalTime localTime = cairoTime.toLocalTime();
LocalDateTime localDateTime = cairoTime.toLocalDateTime();
```

### class DateTimeFormatter

[https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)

- Чтобы создать объект класса `DateTimeFormatter` и <mark class="hltr-yellow">передать в него шаблон, по которому будут отображаться дата и время:</mark>

```java
DateTimeFormatter dtf = DateTimeFormatter.ofPattern(шаблон);
```

```java
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("MM-dd-yy");
String text = dtf.format( LocalDateTime.now() );
System.out.println(text);
```

В метод `format()` можно передать практически любой объект из Date Time API.

| Symbol | Meaning                    | Presentation | Examples                                      |
| ------ | -------------------------- | ------------ | --------------------------------------------- |
| G      | era                        | text         | AD; Anno Domini; A                            |
| u      | year                       | year         | 2004; 04                                      |
| y      | year-of-era                | year         | 2004; 04                                      |
| D      | day-of-year                | number       | 189                                           |
| M/L    | month-of-year              | number/text  | 7; 07; Jul; July; J                           |
| d      | day-of-month               | number       | 10                                            |
| g      | modified-julian-day        | number       | 2451334                                       |
| Q/q    | quarter-of-year            | number/text  | 3; 03; Q3; 3rd quarter                        |
| Y      | week-based-year            | year         | 1996; 96                                      |
| w      | week-of-week-based-year    | number       | 27                                            |
| W      | week-of-month              | number       | 4                                             |
| E      | day-of-week                | text         | Tue; Tuesday; T                               |
| e/c    | localized day-of-week      | number/text  | 2; 02; Tue; Tuesday; T                        |
| F      | day-of-week-in-month       | number       | 3                                             |
| a      | am-pm-of-day               | text         | PM                                            |
| h      | clock-hour-of-am-pm (1-12) | number       | 12                                            |
| K      | hour-of-am-pm (0-11)       | number       | 0                                             |
| k      | clock-hour-of-day (1-24)   | number       | 24                                            |
| H      | hour-of-day (0-23)         | number       | 0                                             |
| m      | minute-of-hour             | number       | 30                                            |
| s      | second-of-minute           | number       | 55                                            |
| S      | fraction-of-second         | fraction     | 978                                           |
| A      | milli-of-day               | number       | 1234                                          |
| n      | nano-of-second             | number       | 987654321                                     |
| N      | nano-of-day                | number       | 1234000000                                    |
| V      | time-zone ID               | zone-id      | America/Los_Angeles; Z; -08:30                |
| v      | generic time-zone name     | zone-name    | Pacific Time; PT                              |
| z      | time-zone name             | zone-name    | Pacific Standard Time; PST                    |
| O      | localized zone-offset      | offset-O     | GMT+8; GMT+08:00; UTC-08:00                   |
| X      | zone-offset 'Z' for zero   | offset-X     | Z; -08; -0830; -08:30; -083015; -08:30:15     |
| x      | zone-offset                | offset-x     | +0000; -08; -0830; -08:30; -083015; -08:30:15 |
| Z      | zone-offset                | offset-Z     | +0000; -0800; -08:00                          |
| p      | pad next                   | pad modifier | 1                                             |
| '      | escape for text            | delimiter    |                                               |
| ''     | single quote               | literal      | '                                             |
| [      | optional section start     |              |                                               |
| ]      | optional section end       |              |                                               |
| #      | reserved for future use    |              |                                               |
| {      | reserved for future use    |              |                                               |
| }      | reserved for future use    |              |                                               |


![[images/Untitled 162.png|Untitled 162.png]]

![[images/Untitled 1 23.png|Untitled 1 23.png]]

![[images/Untitled 2 21.png|Untitled 2 21.png]]

В Java метод `**setLenient(boolean lenient)**` используется для установки режима "безнравственности" в объекте `**DateFormat**`. Если режим "безнравственности" включен (`**lenient**` установлен в `**true**`), то `**DateFormat**` будет пытаться интерпретировать входные данные как дату или время, даже если они находятся вне допустимых пределов. Например, если вы попытаетесь разобрать строку `**"2024-02-30"**`, в режиме "безнравственности" `**SimpleDateFormat**` попытается интерпретировать ее как 2 марта 2024 года, потому что 30 февраля не существует.

Если режим "безнравственности" выключен (`**lenient**` установлен в `**false**`), то `**DateFormat**` будет более строгим в отношении входных данных. В этом режиме он будет проверять, что входные данные соответствуют допустимым датам и времени, и будет выдавать ошибку при попытке разобрать недопустимые данные.

По умолчанию режим "безнравственности" включен в `**SimpleDateFormat**`. Если вам нужен более строгий контроль над разбором дат и времени, вы можете выключить его, вызвав `**setLenient(false)**`.

![[images/Untitled 3 20.png|Untitled 3 20.png]]

  

  

## ==INSTANT== - опорная точка по 1970г.

![[images/Untitled 4 20.png|Untitled 4 20.png]]

![[images/Untitled 5 20.png|Untitled 5 20.png]]

- `Instant` ==показывает точку по== ==UTC== (нулевая зона) , ==не хранит зону==
- `LocalDataTime` ==получаем время и дату непонятно где. И чтобы преобразовать== в `Instant`, ==нужно указать зону, тогда мы получим время по== ==UTC==/

  

![[images/Untitled 6 19.png|Untitled 6 19.png]]

![[images/Untitled 7 18.png|Untitled 7 18.png]]

![[images/Untitled 8 16.png|Untitled 8 16.png]]

![[images/Untitled 9 16.png|Untitled 9 16.png]]
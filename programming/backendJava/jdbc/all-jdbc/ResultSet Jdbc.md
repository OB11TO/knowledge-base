---
title: ResultSet Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 14:40
modified: 2025-04-28T16:15:18+03:00
questions: 
notes: 
links: 
---

-----------
[[RowSet extends ResultSet Jdbc]]




-----


### ResultSet

All Superinterfaces:[AutoCloseable](https://docs.oracle.com/javase/8/docs/api/java/lang/AutoCloseable.html), [Wrapper](https://docs.oracle.com/javase/8/docs/api/java/sql/Wrapper.html)

[https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSet.html](https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSet.html)

<mark class="hltr-red">ResultSet</mark> – это не множество, он просто так называется. <mark class="hltr-green2">В нем хранится результат выполнения запроса</mark>. Этот <mark class="hltr-yellow">объект чем-то похож на итератор: он позволяет устанавливать/менять текущую строку результата, а затем из этой текущей строки можно получить данные.</mark>

ResultSet имеет у себя внутри указатель на текущую строку. И последовательно переключает строки для их чтения с помощью метода next(). Он возвращает true, если такая строка есть, и false, если строки закончились. Результаты как бы обрамлены пустыми строками с обоих сторон. Поэтому изначально текущая строка находится **перед первой** строкой результата. И **чтобы получить первую строку, нужно хотя бы раз вызвать метод next().**

Затем из текущей строки объекта ResultSet можно получить данные из его колонок:

- `getRow()` – возвращает номер текущей строки в объекте ResultSet
- `getInt(N)` – вернет данные N-й колонки текущей строки как тип int
- `getString(N)` – вернет данные N-й колонки текущей строки как тип String

```Java
Statement statement = connection.createStatement();
ResultSet results = statement.executeQuery("SELECT * FROM user");
while (results.next()) {
        	Integer id = results.getInt(1);
        	String name = results.getString(2);
        	System.out.println(results.getRow() + ". " + id + "\t"+ name);
}
```

Если ты **на последней** стоке вызвал метод next(), то ты перешел на строку **после последней**. Данных из нее прочитать ты не можешь, но никакой ошибки не произойдет. Тут метод isAfterLast() будет возвещать true в качестве результата.

|   |   |   |
|---|---|---|
||Метод|Описание|
|1|`next()`|Переключиться на следующую строку|
|2|`previous()`|Переключиться на предыдущую строку|
|3|`isFirst()`|Текущая строка первая?|
|4|`isBeforeFirst()`|Мы перед первой строкой?|
|5|`isLast()`|Текущая строка последняя?|
|6|`isAfterLast()`|Мы после последней сроки?|
|7|`absolute(int n)`|Делает N-ю строку текущей|
|8|`relative(int n)`|Двигает текущую строку на N позиций вперед. N может быть <0|

**Получение данных из текущей строки**

Для этого у объекта ResultSet есть специальные методы, которые все можно описать одним шаблоном:

|   |   |
|---|---|
|SQL Datatype|getXXX() Methods|
|CHAR|`getString()`|
|VARCHAR|`getString()`|
|INT|`getInt()`|
|FLOAT|`getDouble()`|
|CLOB|`getClob()`|
|BLOB|`getBlob()`|
|DATE|`getDate()`|
|TIME|`getTime()`|
|TIMESTAMP|`getTimestamp()`|

- `getType(номерКолонки)`

Впрочем, если у колонки есть имя, то можно получать и по имени колонки:

- `getType(имяКолонки)`

```Java
while (results.next()) {
        	Integer id = results.getInt(“id”);
        	String name = results.getString(“name”);
        	System.out.println(results.getRow() + ". " + id + "\t"+ name);
}
```



------

### Типы ResultSet

![[Pasted image 20250428132635.png]]
- Можем перегружать Statment, чтобы <mark class="hltr-yellow">в RelustSet можно было использовать executeUpdate</mark> <mark class="hltr-red">(но лучше не делать)</mark>, а не только select (query)
- И соответственно, чтобы <mark class="hltr-green2">можно было итерироваться по ResultSet куда угодно, хоть обратно.</mark>
![[Pasted image 20250428133006.png]]

- <mark class="hltr-purple">Также ResultSet можно не закрывать, так как он автоматически закроется от закрытия Statment.</mark>

<mark class="hltr-yellow">ResultSet может быть определенного типа. Тип определяет некоторые характеристики и возможности ResultSet.</mark>

Не все типы поддерживаются всеми базами данных и драйверами JDBC.  
Тебе придется проверить свою базу данных и драйвер JDBC, чтобы увидеть,  
поддерживает ли он тип, который ты хочешь использовать. Метод DatabaseMetaData.supportsResultSetType(int type) возвращает  
_true_ или _false_ в зависимости от того, поддерживается данный тип или нет.

На момент написания статьи существует три типа ResultSet:

- **ResultSet.TYPE_FORWARD_ONLY**
- **ResultSet.TYPE_SCROLL_INSENSITIVE**
- **ResultSet.TYPE_SCROLL_SENSITIVE**

Тип по умолчанию — TYPE_FORWARD_ONLY.

**TYPE_FORWARD_ONLY** означает, что ResultSet можно  
перемещать только вперед. То есть ты можешь перемещаться только из  
строки 1, строки 2, строки 3 и т. д. В ResultSet ты не можешь двигаться  
назад: нельзя считать данные из 9-й строки после чтения десятой.  

**TYPE_SCROLL_INSENSITIVE** означает, что ResultSet  
можно перемещать (прокручивать) как вперед, так и назад. Ты также можешь  
перейти к позиции относительно текущей позиции или перейти к абсолютной  
позиции.  

ResultSet этого типа нечувствителен к  
изменениям в базовом источнике данных, пока ResultSet открыт. То есть  
если запись в ResultSet изменяется в базе данных другим потоком или  
процессом, она не будет отражена в уже открытых ResultSet этого типа.  

**TYPE_SCROLL_SENSITIVE** означает, что ResultSet можно  
перемещать (прокручивать) как вперед, так и назад. Ты также можешь  
перейти к позиции относительно текущей позиции или перейти к абсолютной  
позиции.  

ResultSet этого типа чувствителен к  
изменениям в базовом источнике данных, пока ResultSet открыт. То есть  
если запись в ResultSet изменяется в базе данных другим потоком или  
процессом, она будет отражена в уже открытых ResultSet этого типа.  

  

<mark class="hltr-yellow">Параллельность ResultSet определяет, может ли ResultSet обновляться, или только считываться.</mark>

Некоторые базы данных и драйверы JDBC поддерживают обновление ResultSet, но не все. Метод DatabaseMetaData.supportsResultSetConcurrency(int concurrency) возвращает значение _true_ или _false_ в зависимости от того, поддерживается данный режим параллелизма или нет.

ResultSet может иметь один из двух уровней параллелизма:

- **ResultSet.CONCUR_READ_ONLY**
- **ResultSet.CONCUR_UPDATABLE**

**CONCUR_READ_ONLY** означает, что ResultSet может быть только прочитан.

**CONCUR_UPDATABLE** означает, что ResultSet может быть прочитан и изменен.

  

С помощью этих параметров ты можешь управлять создаваемым Statement и его ResultSet.

Например, можно создать обновляемый ResultSet и с его помощью менять  
базу данных. При создании Statement важно соблюсти следующие условия:  

- указывается только одна таблица
- не содержит предложений join или group by
- столбцы запроса должны содержать первичный ключ

При выполнении вышеуказанных условий обновляемый ResultSet может быть  
использован для модификации таблицы в базе данных. При создании объекта  
Statement нужно указать такие параметры:  

```Java
Statement st = createStatement(Result.TYPE_SCROLL_INSENSITIVE, Result.CONCUR_UPDATABLE)
```

Результатом выполнения такого оператора является обновляемый набор  
результатов. Метод обновления заключается в перемещении курсора  
ResultSet в строку, которую ты хочешь обновить, а затем в вызове метода updateXXX().  

Метод updateXXX работает аналогично методу getXXX(). Метод updateXXX()  
имеет два параметра. Первый — это номер обновляемого столбца, который  
может быть именем столбца или серийным номером. Второй — это данные,  
которые необходимо обновить, и этот тип данных должен быть тот же, что и  
XXX.  

Чтобы строка реально обновилась в базе, нужно вызвать метод updateRow() до того, как курсор ResultSet покинет измененную строку, в противном случае изменения так и не попадут в базу.

Также можно добавлять новые строки в таблицу:

Сначала нужно переместить курсор на пустую строку. Для этого нужно вызвать метод moveToInsertRow().

Затем нужно заполнить эту строку данными с помощью метода updateXXX().

Затем нужно вызвать метод inserRow(), чтобы строка добавилась в базу.

Ну и наконец нужно вернуть курсор обратно, вызвав метод moveToCurrentRow().

-----

- <mark class="hltr-purple"> Можно возвращать автогенерируемый id, чтобы знает его номер например.</mark>
![[Pasted image 20250428133731.png]]
 ![[Pasted image 20250428134016.png]]

-------
![[Pasted image 20250428135145.png]]
![[Pasted image 20250428142859.png]]

-----

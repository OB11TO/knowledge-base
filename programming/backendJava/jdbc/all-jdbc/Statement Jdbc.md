---
title: Statement Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-25 17:53
modified: 2025-04-28T16:18:56+03:00
questions: 
notes: 
links: 
---


### Statement

[https://docs.oracle.com/javase/8/docs/api/java/sql/Statement.html](https://docs.oracle.com/javase/8/docs/api/java/sql/Statement.html)

Интерфейс `Statement` из пакета `java.sql` <mark class="hltr-yellow">представляет собой механизм для отправки SQL-запросов к базе данных и получения результатов.</mark> Этот <mark class="hltr-blue">интерфейс является частью Java Database Connectivity (JDBC) API</mark>, который обеспечивает взаимодействие с различными реляционными базами данных.
   
Вот некоторые основные методы и их описание, доступные в интерфейсе `Statement`:

![[Pasted image 20250425181339.png]]

- `void execute(String sql)`: Выполняет указанный SQL-запрос, который может быть любым допустимым SQL-выражением (например, SELECT, INSERT, UPDATE). Возвращает `true`, если результат запроса является `ResultSet`, и `false` в противном случае. <mark class="hltr-yellow">Лучше всего для DDL операций.</mark>

- `ResultSet executeQuery(String sql)`: Выполняет указанный SQL-запрос, который <mark class="hltr-yellow">должен быть запросом на выборку</mark> (например, SELECT). Возвращает объект `ResultSet`, содержащий результаты запроса.

- `int executeUpdate(String sql)`: Выполняет указанный SQL-запрос, который о<mark class="hltr-yellow">бновляет базу данных</mark> (например, INSERT, UPDATE, DELETE). <mark class="hltr-red">Возвращает количество измененных строк.</mark>

- `boolean isClosed()`: Возвращает `true`, если `Statement` закрыт, и `false` в противном случае.

- `void close()`: Закрывает `Statement` и освобождает все связанные ресурсы.
- `void addBatch(String sql)`: Добавляет SQL-запрос в пакет для пакетной обработки.
- `void clearBatch()`: Очищает все ранее добавленные запросы из пакета.
- `int[] executeBatch()`: Выполняет все запросы в пакете и возвращает массив целых чисел, содержащий результаты для каждого запроса.
- `ResultSet getResultSet()`: Возвращает текущий `ResultSet`, если последний выполненный запрос был запросом на выборку.
- `int getUpdateCount()`: <mark class="hltr-cyan">Возвращает количество измененных строк в последнем выполненном запросе.</mark>

----

### DDL

- в большинстве случаев execute() будет оптимальным вариантом

```Java
public static void main( String[] args ) throws SQLException {
        String sql = "CREATE TABLE IF NOT EXISTS employees (id SERIAL PRIMARY KEY, name VARCHAR(32))";

            try(var connection = ConnectionManager.open();
                var statement = connection.createStatement()){
                    var executeResult = statement.execute(sql);
                    System.out.println(executeResult);
 //false, так как это не SELECT, никаких данных не возвращается
                }
            }
```

### DML

Все DML SQL-запросы можно условно разделить на две группы:

- **Получение данных** — к ним относится оператор SELECT.
- **Изменение данных** — к ним относятся операторы INSERT, UPDATE и DELETE.

Для первой группы используется уже знакомый нам метод интерфейса _Statement_ — `executeQuery()`. Он покрывает очень большой процент запросов.

```Java
String line = "SELECT * from employees WHERE name = 'Vadim';";
try(var connection: Connection = connection.Manager.open();
		 var statement: Statement = connection.createStatement()){
ResultSet set = statement.executeQuery(line);
}
```

Для второй группы запросов нужно использовать другой метод интерфейса Statement — `executeUpdate()`. В отличии от метода `executeQuery()`, который возвращает ResultSet, этот метод возвращает целое число, которое говорит сколько строк в таблице было изменено при исполнении вашего запроса.

```Java
String line = "UPDATE  employee SET salary = salary+1000";
try(var connection: Connection = connection.Manager.open();
		 var statement: Statement = connection.createStatement()){
int updatedCount = statement.executeUpdate(line);
}
```

Могут возникнуть ситуации, когда неизвестно, какой запрос тебе приходится исполнять — выборка или изменение данных. На этот случай создатели JDBC добавили в него еще один универсальный метод — `execute()`.

Метод `execute()` возвращает `boolean`. Если это значение равно _true_, то значит выполнялся запрос на получение данных, и тебе нужно вызвать метод `getResultSet()`.

```Java
boolean hasResults = statement.execute("SELECT Count(*) FROM user"); //true
    if ( hasResults ) {
        	ResultSet results =  statement.getResultSet();
        	results.next();
        	int count = results.getInt(1);
}
```

Если это значение равно `_false_`, то значит выполнялся запрос на изменение данных, и тебе нужно вызвать метод `getUpdateCount()`, чтобы получить количество измененных строк. Пример:

```Java
boolean hasResults = statement //false
.execute("UPDATE  employee SET salary = salary+1000");
if ( !hasResults ) {int count = statement.getUpdateCount();}
```


-----

### Ключевое слово RETURNING

Можно превратить команду UPDATE или INSERT в команду SELECT, чтобы после изменения данных, эти данные возвращались в качестве ResultSet:

![[images/Untitled 3 11.png|Untitled 3 11.png]]

```Java
String sql = "UPDATE buyers SET money = 1000 WHERE name = 'Vadim' or name = 'Dima' RETURNING *;";

        try (var connection = ConnectionManager.open();
             var statement = connection.createStatement()) {
            ResultSet resultSet = statement.executeQuery(sql);
            while (resultSet.next()) {
                System.out.println(resultSet.getString(1) + " " + resultSet.getString(2));
            }
        }

//ВЫВОД
Dima 1000
Dima 1000
Vadim 1000
Vadim 1000
```

![[images/Untitled 4 11.png|Untitled 4 11.png]]

---
title: MetaData Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 16:16
modified: 2025-04-28T16:24:40+03:00
questions: 
notes: 
links: 
---


### MetaData

[https://docs.oracle.com/javase/8/docs/api/java/sql/DatabaseMetaData.html](https://docs.oracle.com/javase/8/docs/api/java/sql/DatabaseMetaData.html)

MetaData: Интерфейс MetaData (иногда называемый DatabaseMetaData) п<mark class="hltr-green2">редоставляет информацию о базе данных в целом, такую как название базы данных, версия СУБД, доступные таблицы и столбцы, поддерживаемые типы данных и т.д. </mark>Он позволяет получить общую информацию о базе данных и ее структуре, но не об отдельных результатов запросов.

Некоторые из методов:

1. `**getSchemas()**`: Возвращает результаты запроса о доступных схемах в базе данных.
2. `**getCatalogs()**`: Возвращает результаты запроса о доступных каталогах в базе данных.
3. `**getProcedureColumns(String catalog, String schemaPattern, String procedureNamePattern, String columnNamePattern)**`: Возвращает результаты запроса о столбцах, связанных с хранимыми процедурами в базе данных.
4. `**getProcedures(String catalog, String schemaPattern, String procedureNamePattern)**`: Возвращает результаты запроса о доступных хранимых процедурах в базе данных.
5. `**getFunctionColumns(String catalog, String schemaPattern, String functionNamePattern, String columnNamePattern)**`: Возвращает результаты запроса о столбцах, связанных с функциями в базе данных.
6. `**getFunctions(String catalog, String schemaPattern, String functionNamePattern)**`: Возвращает результаты запроса о доступных функциях в базе данных.
7. `**getResultSetHoldability()**`: Возвращает уровень сохраняемости результирующего набора (ResultSet) в базе данных.
8. `**getConnection()**`: Возвращает объект Connection, представляющий соединение с базой данных.
9. `**getDatabaseProductName()**`: Возвращает название продукта базы данных.
10. `**getDatabaseProductVersion()**`: Возвращает версию продукта базы данных.
11. `**getDriverName()**`: Возвращает название драйвера базы данных.
12. `**getDriverVersion()**`: Возвращает версию драйвера базы данных.
13. `**getURL()**`: Возвращает URL-адрес базы данных.
14. `**getUserName()**`: Возвращает имя пользователя базы данных.
15. `**supportsResultSetType(int type)**`: Проверяет, поддерживает ли база данных указанный тип результирующего набора.
16. `**supportsTransactionIsolationLevel(int level)**`: Проверяет, поддерживает ли база данных указанный уровень изоляции транзакций.
17. `**getTables(String catalog, String schemaPattern, String tableNamePattern, String[] types)**`: Возвращает результаты запроса о доступных таблицах в базе данных.
18. `**getColumns(String catalog, String schemaPattern, String tableNamePattern, String columnNamePattern)**`: Возвращает результаты запроса о доступных столбцах в таблице.
19. `**getTypeInfo()**`: Возвращает информацию о доступных типах данных в базе данных.
20. `**getPrimaryKeys(String catalog, String schema, String table)**`: Возвращает результаты запроса о первичных ключах для указанной таблицы.
21. `**getImportedKeys(String catalog, String schema, String table)**`: Возвращает результаты запроса об импортированных ключах для указанной таблицы.
22. `**getExportedKeys(String catalog, String schema, String table)**`: Возвращает результаты запроса об экспортированных ключах из указанной таблицы.

## Типы данных, приведение типов, работа со временем

Создатели JDBC начали с того, что зафиксировали список SQL-типов. Есть специальный класс Types с константами: [https://docs.oracle.com/javase/8/docs/api/java/sql/Types.html](https://docs.oracle.com/javase/8/docs/api/java/sql/Types.html)

Число — это не порядковый номер в классе, а ID-тип в списке SQL-типов в спецификации SQL.

```Java
class java.sql.Types {
   public static final int CHAR         =   1;
   public static final int NUMERIC    	=   2;
   public static final int DECIMAL     	=   3;
   public static final int INTEGER      =   4;
   public static final int FLOAT        =   6;
   public static final int REAL         =   7;
  …
}
```

Также в классе ResultSet есть методы, которые умеют преобразовывать  
одни типы данных в другие. Не все типы можно преобразовать друг в друга,  
но логика достаточно понятна. Вот тебе хорошая таблица на первое время:  

|   |   |
|---|---|
|Метод|Тип данных SQL|
|int getInt()|NUMERIC, INTEGER, DECIMAL|
|float getFloat()|NUMERIC, INTEGER, DECIMAL, FLOAT, REAL|
|double getDoublel()|NUMERIC, INTEGER, DECIMAL, FLOAT, REAL|
|Date getDate()|DATE, TIME, TIMESTAMP|
|Time getTime()|DATE, TIME, TIMESTAMP|
|Timestamp getTimestamp()|DATE, TIME, TIMESTAMP|
|String getString()|CHAR, VARCHAR|

----------------

### NULL

Есть таблица в базе денных, которая имеет колонку с каким либо типом, но также может быть и NULL.  
Как получить значение null из этой колонки?  

**Решение первое**. Если SQL-тип в Java представлен ссылочным типом, таким как Date или String, то проблемы нет. Переменные этого типа могут принимать значения null.

**Решение второе**. Примитивные типы не могут принимать значения null, поэтому методы типа getInt() будут просто возвращать значение по умолчанию. Для int это 0, для float = 0.0f, для double = 0.0d и тому подобное.

**Решение третье**. У класса ResultSet есть специальный метод wasNull(), который возвращает true, если только что вместо NULL метод вернул другое значение.

```Java
Connection connection = ConnectionManager.open();

        Statement statement = connection.createStatement();
        ResultSet results = statement.executeQuery("SELECT * FROM buyers");
        results.next();
        String name = results.getString("name");

        if (results.wasNull()) {
            System.out.println("Name is null");
        } else {
            System.out.println("Name is " + name);
```

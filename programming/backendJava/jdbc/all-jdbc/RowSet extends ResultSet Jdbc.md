---
title: RowSet extends ResultSet Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 16:15
modified: 2025-04-28T16:15:28+03:00
questions: 
notes: 
links: 
---

### RowSet extends ResultSet

[https://docs.oracle.com/javase/8/docs/api/javax/sql/RowSet.html](https://docs.oracle.com/javase/8/docs/api/javax/sql/RowSet.html)

Это интерфейс, расширяющий ResultSet и являющийся новым более эффективным для использования.

Основные преимущества:

- RowSet расширяет интерфейс ResultSet, поэтому его функции более мощные, чем у ResultSet.
- RowSet более гибко перемещается по данным таблицы и может прокручивать назад и вперед.
- RowSet поддерживает кэшированные данные, которые можно использовать даже после закрытия соединения.
- RowSet поддерживает новый метод подключения, ты можешь подключиться к базе данных без подключения. Также он поддерживает чтение источника  
    данных XML.  
    
- RowSet поддерживает фильтр данных.
- RowSet также поддерживает операции соединения таблиц.

Типы RowSet:

- **CachedRowSet**
- **FilteredRowSet**
- **JdbcRowSet**
- **JoinRowSet**
- **WebRowSet**

### Создание объекта RowSet

Есть три разных способа получить рабочий объект .

Во-первых, его можно заполнить данными из **ResultSet**, полученного классическим способом.

Например, мы можем закешировать данные **ResultSet** с помощью **CachedRowSet**:

```Java
Statement statement = connection.createStatement();
ResultSet results = statement.executeQuery("SELECT * FROM user");

RowSetFactory factory = RowSetProvider.newFactory();
CachedRowSet crs = factory.createCachedRowSet();
crs.populate(results);		// Используем ResultSet для заполнения
```

Во-вторых, можно создать полностью автономный объект **RowSet**, создав для него свое собственное подключение к базе данных:

```Java
JdbcRowSet rowSet = RowSetProvider.newFactory().createJdbcRowSet();
rowSet.setUrl("jdbc:mysql://localhost:3306/test");
rowSet.setUsername("root");
rowSet.setPassword("secret");

rowSet.setCommand("SELECT * FROM user");
rowSet.execute();
```

И в-третьих, можно подключить RowSet к уже существующему соединению:

```Java
Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "root", "root");JdbcRowSet rowSet = new JdbcRowSetImpl(con);rowSet.setCommand("SELECT * FROM user");rowSet.execute();
```

### Примеры работы с RowSet

Пример первый: **кэширование**.

Давай напишем код, где с помощью **CachedRowSet** закешируем все данные и будем читать их из уже закрытого соединения:

```Java
Statement statement = connection.createStatement();
ResultSet results = statement.executeQuery("SELECT * FROM user");

RowSetFactory factory = RowSetProvider.newFactory();
CachedRowSet crs = factory.createCachedRowSet();
crs.populate(results);	// Используем ResultSet для заполнения

connection.close();		// Закрываем соединение// Данные из кэша все еще доступныwhile(crs.next()) {
  	System.out.println(crs.getString(1)+"\t"+ crs.getString(2)+"\t"+ crs.getString(3));
}
```

Пример второй: **изменение строк через RowSet**:

```Java
// Подключаемся к базе данных
 CachedRowSet crs = rsf.createCachedRowSet();
 crs.setUrl("jdbc:mysql://localhost/test");
 crs.setUsername("root");
 crs.setPassword("root");
 crs.setCommand("SELECT * FROM user");
 crs.execute();

// Этот тип операции может изменить только автономный RowSet
// Сначала перемещаем указатель на пустую (новую) строку, текущая позиция запоминается
 crs.moveToInsertRow();
 crs.updateString(1, Random.nextInt());
 crs.updateString(2, "Клон" + System.currentTimeMillis());
 crs.updateString(3, "Женский");
 crs.insertRow();  // Добавляем текущую (новую) строку к остальные строкам
 crs.moveToCurrentRow(); // Возвращаем указатель на ту строку, где он был до вставки

 crs.beforeFirst();
 while(crs.next()) {
 	System.out.println(crs.getString(1) + "," + crs.getString(2) + "," + crs.getString(3));
}

// А теперь мы можем все наши изменения залить в базу
Connection con = DriverManager.getConnection("jdbc:mysql://localhost/dbtest", "root", "root");
con.setAutoCommit(false); // Нужно для синхронизации
crs.acceptChanges(con);// Синхронизировать данные в базу данных
```

### ResultSetMetaData

[https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSetMetaData.html](https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSetMetaData.html)

ResultSetMetaData: Интерфейс ResultSetMetaData используется для получения информации о метаданных конкретного результирующего набора (ResultSet) после выполнения запроса к базе данных. Он предоставляет информацию о столбцах в результирующем наборе, такую как их имена, типы данных, размеры и т.д. С помощью ResultSetMetaData можно получить информацию о структуре данных, содержащихся в результирующем наборе, и использовать эту информацию для динамической обработки результатов запроса.

- `getCatalogName(int column):` Возвращает имя каталога таблицы, к которой относится указанный столбец.
- `getColumnClassName(int column)`: Возвращает полное квалифицированное имя класса Java, объекты которого создаются при вызове метода ResultSet.getObject для извлечения значения из столбца.
- `getColumnCount()`: Возвращает количество столбцов в этом объекте ResultSet.
- `getColumnDisplaySize(int column)`: Возвращает нормальную максимальную ширину указанного столбца в символах.
- `getColumnLabel(int column)`: Получает предлагаемый заголовок для указанного столбца, который может использоваться при выводе на печать или отображении.
- `getColumnName(int column)`: Возвращает имя указанного столбца.
- `getColumnType(int column)`: Возвращает SQL-тип указанного столбца.
- `getColumnTypeName(int column)`: Возвращает имя типа указанного столбца в базе данных.
- `getPrecision(int column)`: Возвращает указанный размер столбца.
- `getScale(int column)`: Получает количество десятичных знаков справа от десятичной точки в указанном столбце.
- `getSchemaName(int column)`: Возвращает схему таблицы, к которой относится указанный столбец.
- `getTableName(int column)`: Возвращает имя таблицы указанного столбца.
- `isAutoIncrement(int column)`: Указывает, является ли указанный столбец автоматически нумеруемым.
- `isCaseSensitive(int column)`: Указывает, имеет ли регистр значения столбца значение.
- `isCurrency(int column)`: Указывает, является ли указанный столбец денежным значением.
- `isDefinitelyWritable(int column)`: Указывает, будет ли успешно выполнена запись в указанный столбец.
- `isNullable(int column)`: Указывает возможность присутствия значений null в указанном столбце.
- `isReadOnly(int column)`: Указывает, является ли указанный столбец определенно нередактируемым.
- `isSearchable(int column)`: Указывает, может ли указанный столбец быть использован в предложении WHERE.
- `isSigned(int column)`: Указывает, являются ли значения в указанном столбце знаковыми числами.
- `isWritable(int column)`: Указывает, может ли произойти успешная запись в указанный столбец.

Пример получения метаданных:

```Java
ResultSetMetaData metaData = results.getMetaData();
int columnCount = metaData.getColumnCount();
for (int column = 1; column <= columnCount; column++)
{
        	String name = metaData.getColumnName(column);
        	String className = metaData.getColumnClassName(column);
        	String typeName = metaData.getColumnTypeName(column);
        	int type = metaData.getColumnType(column);

        	System.out.println(name + "\t" + className + "\t" + typeName + "\t" + type);


//ВЫВОД:
id	java.lang.Integer	INT	4
name	java.lang.String	VARCHAR	12
level	java.lang.Integer	INT	4
created_date	java.sql.Date	DATE	91
```

  

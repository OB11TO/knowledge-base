---
title: BatchSize Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 16:31
modified: 2025-04-29T14:25:23+03:00
questions: 
notes: 
links: 
---


### Batching запросов

В реальных проектах часто возникает ситуация, когда необходимо сделать очень много однотипных запросов (наиболее часто в этом случае  
встречается  
_PreparedStatement_), например, надо вставить несколько десятков или сотен записей.

Если выполнять каждый запрос отдельно, то это займет кучу времени и снизит производительность приложения. Чтобы не допустить этого, можно  
использовать batch-режим вставки. Он заключается в том, что ты накапливаешь некоторый буфер своими запросами, а потом выполняешь их сразу.  

  

```Java
PreparedStatement stmt = con.prepareStatement(
        	"INSERT INTO jc_contact (first_name, last_name, phone, email) VALUES (?, ?, ?, ?)");

for (int i = 0; i < 10; i++) {
	// Заполняем параметры запроса
	stmt.setString(1, "FirstName_" + i);
    stmt.setString(2, "LastNAme_" + i);
    stmt.setString(3, "phone_" + i);
    stmt.setString(4, "email_" + i);
	// Запрос не выполняется, а укладывается в буфер,
	//  который потом выполняется сразу для всех команд
	stmt.addBatch();
}
// Выполняем все запросы разом
int[] results = stmt.executeBatch();
```

==**ВАЖНО**==: в данном примере выполняется однотипный запрос через PreparedStatement. Когда несколько разных PreparedStatement с разными запросами использовать batching не получится, придется использовать обычный Statement, в который можно сразу будет передавать строку и добавлять statement в пул.

==ВАЖНО==: важен порядок добавления в батч запросов, которые связаны внешники ссылками.

```Java
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

public class TransactionRunner {
    public static void main(String[] args) {
        int id = 10;
        try {
            deleteEmployee(id);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public static void deleteEmployee(int id) throws SQLException {
        var deleteEmployeeSql = "DELETE FROM employees WHERE salary = " +id;
        var deletePassport = "DELETE FROM passports WHERE salary = "+id;
        Connection connection = null;
        Statement statement = null;
        try{
            connection = ConnectionManager.open();
            connection.setAutoCommit(false);
            statement = connection.createStatement();

            statement.addBatch(deletePassport);
            statement.addBatch(deleteEmployeeSql);
            int[] ints = statement.executeBatch();
        }
        catch (Exception e){
            if(connection!= null){
                connection.rollback();
            }
            throw e;
        }
        finally {
            if(connection != null){
                connection.close();
            }
            if(statement!= null){
                statement.close();
            }
        }
    }
}
```

Вместо того, чтобы выполнять запрос методом execute(), мы складываем его в пакет с помощью метода addBatch().

А затем, когда набралось несколько сотен запросов, можно их разом отправить на сервер, вызвав команду executeBatch().

**Полезно.** Метод executeBatch() возвращает массив целых чисел — int[]. Каждая ячейка этого массива содержит число, которое означает количество строк, измененных соответствующим запросом. Если запрос номер 3 в batch’е изменил 5 строк, то 3-я ячейка массива будет содержать число 5.

-----
## Что такое fetchSize

### Определение

- `fetchSize` — это число строк, которые JDBC-драйвер за один раз извлекает из базы при обработке `ResultSet` [Stack Overflow](https://stackoverflow.com/questions/1318354/what-does-statement-setfetchsizensize-method-really-do-in-sql-server-jdbc-driv?utm_source=chatgpt.com).
    
- Если не вызывать `setFetchSize()`, драйвер использует значение по умолчанию, часто это 10 или неограниченное число, в зависимости от реализации [Oracle Docs](https://docs.oracle.com/javase/8/docs/api/java/sql/Statement.html?utm_source=chatgpt.com).
    

### Зачем нужен

- Уменьшает число сетевых раунд-трипов: при большом `fetchSize` за один вызов прилетает больше строк, и, наоборот, при малом — частые обращения к БД [GeeksforGeeks](https://www.geeksforgeeks.org/fetch-large-result-sets-efficiently-using-fetch-size-in-jdbc/?utm_source=chatgpt.com).
    
- Контролирует потребление памяти в клиенте: большие `fetchSize` требуют больше оперативки для буферизации строк [in.relation.to](https://in.relation.to/2025/01/24/jdbc-fetch-size/?utm_source=chatgpt.com).
    

---

## Что такое batchSize

### Определение

- `batchSize` — это число SQL-запросов (INSERT/UPDATE/DELETE), которые накапливаются в JDBC-клиенте (или в Hibernate) и отправляются пакетом одной командой `executeBatch()` [Baeldung](https://www.baeldung.com/jdbc-batch-processing?utm_source=chatgpt.com).
    
- В Spark-коннекторах и других фреймворках это тоже параметр, определяющий число строк записи за один раунд-трип [Apache Spark](https://spark.apache.org/docs/preview/sql-data-sources-jdbc.html?utm_source=chatgpt.com).
    

### Зачем нужен

- Снижает накладные расходы на отправку каждого DML-запроса: вместо N отдельных сетевых запросов выполняется ⌈N/batchSize⌉ пакетов [Stack Overflow](https://stackoverflow.com/questions/66356929/how-to-select-optimal-batch-size-in-jdbc?utm_source=chatgpt.com).
    
- Позволяет базе оптимизировать групповые вставки/обновления, используя внутренние механизмы batch-processing [Baeldung](https://www.baeldung.com/spring-jdbc-batch-inserts?utm_source=chatgpt.com).
    

---

## Отличия между fetchSize и batchSize

|Параметр|Операция|Что настраивает|Влияет на|
|---|---|---|---|
|fetchSize|SELECT (чтение)|Число строк за один раунд-трип при извлечении|Число сетевых вызовов, память|
|batchSize|INSERT/UPDATE/DELETE (запись)|Число DML-операций в одном пакете|Число пакетов на запись, CPU/IO|

- **Сценарий**: `fetchSize` для больших результатов выборки, `batchSize` — для массовых операций записи.
    
- **Ошибки**: смешение понятий приведёт к тому, что вы попытаетесь «пакетировать» `SELECT` или «фетчить» `INSERT`, что не имеет смысла.
    

---

## Влияние на производительность

### fetchSize

- Малое значение (например, 10) — много сетевых раунд-трипов, низкая задержка на клиенте, но высокие накладные расходы на сеть [Stack Overflow](https://stackoverflow.com/questions/1318354/what-does-statement-setfetchsizensize-method-really-do-in-sql-server-jdbc-driv?utm_source=chatgpt.com).
    
- Большое значение (тысячи) — меньше раунд-трипов, но больше памяти на клиенте; есть риск OOM при экстремальных значениях [GeeksforGeeks](https://www.geeksforgeeks.org/fetch-large-result-sets-efficiently-using-fetch-size-in-jdbc/?utm_source=chatgpt.com).
    
- Оптимум зависит от пропускной способности сети, размера строк и доступной памяти.
    

### batchSize

- Малое (10–50) — много отдельных команд, низкий выигрыш в пропускной способности.
    
- Оптимальное (50–200) — часто рекомендуемое для Oracle и PostgreSQL: баланс памяти и скорости [Stack Overflow](https://stackoverflow.com/questions/66356929/how-to-select-optimal-batch-size-in-jdbc?utm_source=chatgpt.com).
    
- Слишком большое (>1000) — растёт память на клиенте и сервере, падает отдача, возможны таймауты или lock-конкуренция [Medium](https://medium.com/decathlondigital/how-to-optimize-database-configuration-for-batching-a6dac24b05e5?utm_source=chatgpt.com).
    

---

## Рекомендации по настройке

|Параметр|Рекомендуемое значение|Как задать|
|---|---|---|
|fetchSize|от 100 до 1000 (тестировать)|`statement.setFetchSize(n);` или `hibernate.jdbc.fetch_size` [Oracle Docs](https://docs.oracle.com/middleware/1212/toplink/TLJPA/q_jdbc_fetch_size.htm?utm_source=chatgpt.com)|
|batchSize|50–200 (по умолчанию 100)|`preparedStatement.addBatch();` + `preparedStatement.executeBatch();` или `hibernate.jdbc.batch_size` [Stack Overflow](https://stackoverflow.com/questions/21257819/what-is-the-difference-between-hibernate-jdbc-fetch-size-and-hibernate-jdbc-batc?utm_source=chatgpt.com)|

1. **Измеряйте**: прогоняйте нагрузочные тесты с разными значениями.
    
2. **Мониторьте**: смотрите на сетевые вызовы, GC и потребление памяти.
    
3. **Учтите БД**: Oracle по умолчанию фетчит по 10 строк, PostgreSQL — все сразу; batch-поведение тоже зависит от драйвера [Stack Overflow](https://stackoverflow.com/questions/70020359/can-setting-higher-value-of-jdbc-fetch-size-affect-the-database-performance?utm_source=chatgpt.com)​[Apache Spark](https://spark.apache.org/docs/preview/sql-data-sources-jdbc.html?utm_source=chatgpt.com).

----

![[Pasted image 20250428165155.png]]

----

![[Pasted image 20250429142512.png]]



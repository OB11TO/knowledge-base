---
title: Транзакции в Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 16:41
modified: 2025-04-28T16:42:14+03:00
questions: 
notes: 
links: 
---

### Транзакции в jdbc

Каждый вызов метода execute() объекта Statement выполняется в отдельной транзакции. Для этого у Connection есть параметр AutoCommit. Если он выставлен в _true_, то commit() будет вызываться после каждого вызова метода execute().

Если ты хочешь выполнить несколько команд в одной транзакции, то сделать это можно так:

- отключаем AutoCommit
- вызываем наши команды
- вызываем метод commit() явно

```Java
connection.setAutoCommit(false);

Statement statement = connection.createStatement();
int rowsCount1 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount2 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount3 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");

connection.commit();
```

Если во время работы метод commit() на сервере произойдет ошибка, то SQL-сервер отменит все три действия.

Но бывают ситуации, когда ошибка возникает еще на стороне клиента, и мы так и не дошли до вызова метода commit():

```Java
connection.setAutoCommit(false);

Statement statement = connection.createStatement();
int rowsCount1 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount2 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
int rowsCount3 = statement.executeUpdate("UPDATE  несколько опечаток приведут к исключению");

connection.commit();
```

Если во время работы одного executeUpdate() произойдет ошибка, то метод commit() вызван так и не будет. Чтобы откатить все сделанные действия, нужно вызвать метод rollback().

Есть две таблицы employees и passports @One to many. Удалить запись из первой без удаления соответствующей записи из второй нельзя. Решение через транзакции:

```Java
package ru.konovalov;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TransactionRunner {
    public static void main(String[] args) {
        int id = 10;
        try {
            deleteEmployee(id);
        } catch (SQLException e) {
            System.out.println(e.printStackTrace(););
        }
    }
    
    public static void deleteEmployee(int id) throws SQLException {
        var deleteEmployeeSql = "DELETE FROM employees WHERE salary = ?";
        var deletePassport = "DELETE FROM passports WHERE salary = ?";
        Connection connection = null;
        PreparedStatement deletePassStat = null;
        PreparedStatement deleteEmployeeStat = null;
        try{
            connection = ConnectionManager.open();
            connection.setAutoCommit(false);
            deleteEmployeeStat = connection.prepareStatement(deleteEmployeeSql);
            deletePassStat = connection.prepareStatement(deletePassport);
            deleteEmployeeStat.setLong(1,id);
            deletePassStat.setLong(1,id);
            
            deleteEmployeeStat.executeUpdate();
            deletePassStat.executeUpdate();
            
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
            if(deleteEmployeeStat!= null){
                deleteEmployeeStat.close();
            }
            if(deletePassStat!= null){
                deletePassStat.close();
            }
        }
    }
}
```

### Точки сохранения

Для того, чтобы сохраниться, нужно создать точку сохранения, делается это командой:

`Savepoint save = connection.setSavepoint();`

Возврат к точке сохранения делается командой:

`connection.rollback(save);`

```Java
try{
  	connection.setAutoCommit(false);

  	Statement statement = connection.createStatement();
  	int rowsCount1 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");
  	int rowsCount2 = statement.executeUpdate("UPDATE  employee SET salary = salary+1000");

  	Savepoint save = connection.setSavepoint();
 	 try{
      	int rowsCount3 = statement.executeUpdate("UPDATE  несколько опечаток приведут к исключению");
 	 }
 	 catch (Exception e) {
    	   connection.rollback(save);
 	 }

	  connection.commit();
 }
 catch (Exception e) {
   connection.rollback();
}
```


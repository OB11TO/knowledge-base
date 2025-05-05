---
title: FetchSize Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 16:11
modified: 2025-04-28T16:23:07+03:00
questions: 
notes: 
links: 
---

### FetchSize

![[Pasted image 20250428161201.png]]

Метод `setFetchSize()` используется <mark class="hltr-green2">для установки размера выборки (fetch size) при извлечении данных из базы данных с использованием ResultSet.</mark> <mark class="hltr-yellow">Размер выборки определяет количество строк, которые будут извлечены за одну операцию из базы данных и переданы клиентскому приложению.</mark>

Метод `setQueryTimeout(int seconds)` <mark class="hltr-red">используется для установки таймаута выполнения SQL - запроса в секундах. Если операция выполнения запроса занимает больше времени, чем указанное значение таймаута, то будет сгенерировано исключение</mark> `SQLException`.

Метод `setMaxRows(int max)` <mark class="hltr-green2">используется для установки  
максимального числа строк, которые будут возвращены в результирующем  
наборе при выполнении SQL - запроса</mark>. Если запрос возвращает больше строк,  
чем указанное значение  
`max`, то оставшиеся строки будут игнорироваться.

![[Pasted image 20250428162257.png]]

```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class FetchSizeExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String username = "root";
        String password = "password";

        try (Connection connection = DriverManager.getConnection(url, username, password);
             Statement statement = connection.createStatement()) {

	          // Установка размера выборки
	         int fetchSize = 100; // Устанавливаем размер выборки равным 100
            statement.setFetchSize(fetchSize);

            // Установка таймаута запроса в 30 секунд
            int queryTimeout = 30;
            statement.setQueryTimeout(queryTimeout);

						// Установка максимального числа строк равным 10
            int maxRows = 10;
            statement.setMaxRows(maxRows);

            String sql = "SELECT * FROM users";
            ResultSet resultSet = statement.executeQuery(sql);

            // Обработка результата
            while (resultSet.next()) {
                // Чтение данных из результирующего набора
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                // Дополнительная обработка данных
                System.out.println("User ID: " + id + ", Name: " + name);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

  

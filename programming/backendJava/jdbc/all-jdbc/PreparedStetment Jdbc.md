---
title: PreparedStetment Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 14:01
modified: 2025-04-28T16:09:40+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20250428140106.png]]


- <mark class="hltr-purple">В отличии от Statment, здесь нужно сразу при создании передавать sql </mark>
	<mark class="hltr-green2">Предварительная компиляция:</mark> PreparedStatement <mark class="hltr-yellow">компилирует SQL-запрос на стороне базы данных при его создании.</mark> При выполнении запроса база данных может повторно использовать скомпилированный план выполнения, что приводит к более эффективной обработке запросов. ==В случае обычного Statement каждый sql запрос компилируется== непосредственно перед выполнением, что может вызывать накладные расходы на компиляцию для каждого запроса.
![[Pasted image 20250428142126.png]]

<mark class="hltr-green2">Параметризованные запросы</mark>: PreparedStatement позволяет использовать параметры в SQL-запросах. Параметры представляются в виде плейсхолдеров (например, "?") и могут быть заполнены значениями во время выполнения. Это позволяет безопасно передавать пользовательский ввод в запросы, предотвращая атаки типа ==**SQL-инъекции.

==Улучшенная производительность:== При использовании PreparedStatement база данных ==может кэшировать скомпилированные планы выполнения запросов==, что приводит к более быстрой обработке последующих запросов. Это особенно полезно, если вы выполняете один и тот же запрос с различными значениями параметров.

----
```java
// Переменные для примера
String firstName = "Harry";
String lastName = "Potter";
String phone = "+12871112233";
String email = "harry@example.com";

// Запрос с указанием мест для параметров в виде знака "?"
String sql = "INSERT INTO JC_CONTACT
(FIRST_NAME, LAST_NAME, PHONE, EMAIL)
VALUES (?, ?, ?, ?)";

// Создание запроса. Переменная con — это объект типа Connection
PreparedStatement stmt = con.prepareStatement(sql);

// Установка параметров
stmt.setString(1, firstName);
stmt.setString(2, lastName);
stmt.setString(3, phone);
stmt.setString(4, email);

// Выполнение запроса
stmt.executeUpdate();
```

********

```Java
public class JDBCRunner {
    public static void main(String[] args) throws SQLException {
        LocalDateTime start  = LocalDateTime.of(2023, Month.AUGUST, 15, 12, 0);
        LocalDateTime end  = LocalDateTime.of(2023, Month.SEPTEMBER, 1, 0, 0);
        List<Long> listOfFlights = getAllFlights(start,end);
    }

    private static List<Long> getAllFlights(LocalDateTime start, LocalDateTime end) throws SQLException {
        String sql = "SELECT * FROM tickets WHERE departure_date BETWEEN ? AND ?";

        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(sql)) {
            statement.setTimestamp(1, Timestamp.valueOf(start));
            statement.setTimestamp(1, Timestamp.valueOf(start));
            ResultSet resultSet = statement.executeQuery();
            List<Long> list = new ArrayList<>();
            while (resultSet.next()){
                list.add(resultSet.getLong("id"));
            }
            return list;
        }
    }
}
```

------------

![[Pasted image 20250428160930.png]]

---
title: CallableStatement Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 14:37
modified: 2025-04-28T14:39:47+03:00
questions: 
notes: 
links: 
---

### CallableStatement extends PreparedStatement

[https://docs.oracle.com/javase/8/docs/api/java/sql/CallableStatement.html](https://docs.oracle.com/javase/8/docs/api/java/sql/CallableStatement.html)

JDBC есть еще один интерфейс для еще более сложных сценариев. Он унаследован от _PreparedStatement_ и называется _CallableStatement_.

Он<mark class="hltr-green2"> используется для вызова (Call) хранимых процедур в базе данных</mark>. Особенность такого вызова в том, что <mark class="hltr-yellow">кроме результата ResultSet такой хранимой процедуре можно еще и передать параметры.</mark>

Для создания объекта _CallableStatement_ предназначен метод <mark class="hltr-red">Connection.prepareCall().</mark>

Вот представь, что у тебя есть хранимая процедура ADD, которая  
принимает параметры a, b и c. Эта процедура складывает a и b и помещает  
результат сложения в переменную с.  

```Java
// Подключение к серверу
Connection connection = DriverManager.getConnection("jdbc:as400://mySystem");

// Создание объекта CallableStatement. Он выполняет предварительную обработку
// вызова хранимой процедуры. Знаки вопроса
// указывают, где должны быть подставлены входные параметры, а где выходные
// Первые два параметра являются входными,
// а третий — выходным.
CallableStatement statement = connection.prepareCall("CALL MYLIBRARY.ADD (?, ?, ?)");

// Настройка входных параметров. Передаем в процедуру 123 и 234
statement.setInt (1, 123);
statement.setInt (2, 234);

// Регистрация типа выходного параметра
statement.registerOutParameter (3, Types.INTEGER);

// Запуск хранимой процедуры
statement.execute();

// Получение значения выходного параметра
int sum = statement.getInt(3);

// Закрытие CallableStatement и Connection
statement.close();
connection.close();
```

Работа почти как с _PreparedStatement_, только есть нюанс. Наша функция ADD возвращает в третьем параметре результат сложения. Только вот объект _CallableStatement_ об этом ничего не знает. Поэтому мы говорим ему об это явно, вызвав метод registerOutParameter():

`registerOutParameter(номерПараметра, типПараметра)`

После этого можно вызывать процедуру через метод execute() и затем читать данные из третьего параметра с помощью методa getInt().

-----

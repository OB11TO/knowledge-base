---
title: Теория про DAO Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 17:59
modified: 2025-04-29T14:54:31+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20250428180716.png]]

## DAO

Пример использования в проекте: [https://github.com/Onemyname/ConsulApp/tree/master/src/main/java/ru/konovalov](https://github.com/Onemyname/ConsulApp/tree/master/src/main/java/ru/konovalov)

В контексте JDBC (Java Database Connectivity),<mark class="hltr-red"> DAO означает Data Access Object (Объект доступа к данным).</mark> <mark class="hltr-green2">DAO является шаблоном проектирования, используемым для организации доступа к данным в приложениях Java.</mark>

<mark class="hltr-yellow">DAO позволяет отделить бизнес-логику приложения от механизмов доступа к данным, что упрощает разработку и поддержку кода.</mark> Он о<mark class="hltr-green2">беспечивает абстракцию уровня доступа к данным, что позволяет изменять и заменять источник данных без влияния на остальные части приложения.</mark>

В JDBC DAO позволяет выполнять операции с базой данных, используя JDBC API. Он может содержать методы для создания соединения с базой данных, выполнения SQL-запросов, извлечения и обработки результатов запросов и управления транзакциями.

- <mark class="hltr-purple">DAO Синглтоны</mark>
----
 ![[Pasted image 20250428181019.png]]


![[Pasted image 20250428181305.png]]


---

![[Pasted image 20250429135838.png]]
## Когда использовать

- **Stateful**: многошаговые процессы (бронирование, корзина), когда нужно хранить промежуточные данные между вызовами [coderanch.com](https://coderanch.com/t/597281/java/Stateless-Stateful-bean?utm_source=chatgpt.com).
    
- **Stateless**: CRUD-сервисы, справочные операции, когда каждый запрос самодостаточен и масштабируется горизонтально [Baeldung](https://www.baeldung.com/cs/stateful-stateless-system?utm_source=chatgpt.com).

-----

пример с DAO

Интерфейс, который будут реализовать все DAO:

```Java
package ru.konovalov;

import java.util.List;
import java.util.Optional;

public interface DAO<T,E> {
    String save(E clazz);
    boolean delete(T id);
    boolean update(E clazz);
    List<E> findAll();
    Optional<E> findById(T id);
}
```

Добавляем доп сущности:

```Java
package ru.konovalov;

public record Passport(int id, String number) {
}
```

```Java
package ru.konovalov;

import java.sql.SQLException;
import java.util.List;
import java.util.Optional;

public class PassportDAO implements DAO<Integer,Passport> {
    private static final PassportDAO INSTANCE = new PassportDAO();
    private static final String FIND_BY_ID_SQL = """
            SELECT id, number FROM passports
            WHERE id=?
            """;

    private PassportDAO(){}
    public static PassportDAO getInstance(){
        return INSTANCE;
    }

@Override
    public Optional<Passport> findById(Integer id) {
        try (var conection = ConnectionManager.open()){
            return findById(id,conection);
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public Optional<Passport> findById(Integer id, Connection connection) {
        try (var statement = connection.prepareStatement(FIND_BY_ID_SQL)) {
            statement.setInt(1,id);
            var resultSet = statement.executeQuery();
            Passport passport = null;
            if(resultSet.next()){
                passport = new Passport(
                        resultSet.getInt("id"),
                        resultSet.getString("number")
                );
            }
            return Optional.ofNullable(passport);
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }
}
```

Меняем build в классе consumerDAO:

```Java
private Consumer build(ResultSet resultSet) throws SQLException {
        return new Consumer(
                resultSet.getInt("id"),
                resultSet.getString("first_name"),
                resultSet.getString("last_name"),
PASSPORT_DAO.findById(resultSet.getInt("passport"), resultSet.getStatement().getConnection()).orElse(null),
                resultSet.getInt("salary"));
    }
```

```Java
System.out.println(getConsumerById(4));
Optional[Consumer(id=4, firstName=Dmitriy, lastName=Parfen4ik, passport=Passport[id=2, number=753848], salary=3000)]
```

ВАЖНО: в данном примере мы не открываем новое соединение для поиска Passport, а переиспользуем существующее, используя перегрузку.



-----

### Сложный EntryMapping

В предыдущеим примере Consumer содержал поле int passport, но в моих таблицах это FOREIGN KEY на `таблицу passports @One to One. Ниже приведено как реализовать, чтобы при выполнении был не int, а отдельная сущность Passport.

### 1 пример с JOIN

![[images/Untitled 5 11.png|Untitled 5 11.png]]

![[images/Untitled 6 10.png|Untitled 6 10.png]]

```Java
package ru.konovalov;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class ConsumerDAO {
    private static final ConsumerDAO INSTANCE = new ConsumerDAO();
    private static final String INSERT_SQL = "INSERT INTO consumers (first_name, last_name, passport, salary)" +
                                             " VALUES(?,?,?,?)";
    private static final String DELETE_SQL = "DELETE FROM consumers WHERE id = ?";
    private static final String UPDATE_SQL = "UPDATE consumers" +
                                             " SET first_name =?, last_name = ?, passport = ?, salary = ?" +
                                             " WHERE id = ?";

    private static final String FIND_ALL_SQL = "SELECT c.id, first_name, last_name, passport, salary, p.number " +
                                               "FROM consumers c " +
                                               "JOIN passports p on p.id = c.passport";

    private static final String FIND_BY_ID = FIND_ALL_SQL + " WHERE c.id = ?";

    private ConsumerDAO() {
    }

    public static ConsumerDAO getInstance() {
        return INSTANCE;
    }

    public String save(Consumer consumer) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(INSERT_SQL, Statement.RETURN_GENERATED_KEYS)) {
            statement.setString(1, consumer.getFirstName());
            statement.setString(2, consumer.getLastName());
            statement.setInt(3, consumer.getPassport().id());
            statement.setInt(4, consumer.getSalary());
            int executed = statement.executeUpdate();
            var generatedKeys = statement.getGeneratedKeys();
            generatedKeys.next();
            consumer.setId(generatedKeys.getInt("id"));
            return "Consumer is saved! id = " + consumer.getId();
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public boolean delete(int id) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(DELETE_SQL, Statement.RETURN_GENERATED_KEYS)) {
            statement.setInt(1, id);
            int executed = statement.executeUpdate();
            return executed > 0;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public boolean update(Consumer consumer) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(UPDATE_SQL)) {
            statement.setString(1, consumer.getFirstName());
            statement.setString(2, consumer.getLastName());
            statement.setInt(3, consumer.getPassport().id());
            statement.setInt(4, consumer.getSalary());
            statement.setInt(5, consumer.getId());

            return statement.executeUpdate() > 0;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public List<Consumer> getAllConsumers() {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(FIND_ALL_SQL)) {
            ResultSet resultSet = statement.executeQuery();
            List<Consumer> consumers = new ArrayList<>(10);
            while (resultSet.next()) {
                consumers.add(build(resultSet));
            }
            return consumers;
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    public Optional<Consumer> findConsumerById(int id) {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(FIND_BY_ID)) {
            statement.setInt(1, id);
            ResultSet resultSet = statement.executeQuery();
            Consumer consumer = null;
            if (resultSet.next()) {
                consumer = build(resultSet);
            }
            return Optional.ofNullable(consumer);
        } catch (SQLException e) {
            throw new DAOException(e);
        }
    }

    private Consumer build(ResultSet resultSet) throws SQLException {
        var passport = new Passport(resultSet.getInt("id"),
                resultSet.getString("number"));
        return new Consumer(
                resultSet.getInt("id"),
                resultSet.getString("first_name"),
                resultSet.getString("last_name"),
                passport,
                resultSet.getInt("salary"));
    }

}
```

```Java
List<Consumer> list = getAllConsumers();
list.stream().forEach(System.out::println);
Consumer(id=2, firstName=Vadim, lastName=Konovalski, passport=Passport[id=2, number=7584848], salary=1000)
Consumer(id=4, firstName=Dmitriy, lastName=Parfen4ik, passport=Passport[id=4, number=753848], salary=3000)
```

Минусы такого подохода:

- ConsumerDao внутри метода build() должен знать как билдить объект типа Passport.
- В моем случае есть дополнительная сущность Passport, которая содержит только number, а если она будет содержать внутри себя еще сущности,а они будут содержать другие сущности? По-хорошему придется делать большой JOIN и билдить большое количество объектов в одном методе. Понимание кода усложнится.

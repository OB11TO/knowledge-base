---
title: Blob and Clob Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-28 16:55
modified: 2025-04-28T17:26:38+03:00
questions: 
notes: 
links: 
---

![[Pasted image 20250428172351.png]]
- <mark class="hltr-red">В pg есть ток такие </mark>
### Blob

Если ты хочешь<mark class="hltr-yellow"> сохранить какой-то объект в базу данных, то самый  
простой способ сделать это — воспользовавшись SQL-типом BLOB</mark>. У JDBC  
есть его аналог, который называется Blob.  

BLOB расшифровывается как **Binary Large Objec t**.  
<mark class="hltr-green2">Он используется для хранения массива байт.</mark> Тип Blob в JDBC является  
интерфейсом и в него можно класть (и получать) данные двумя способами:  

- С помощью InputStream
- С помощью массива байт

Пример: колонка номер 3 содержит тип BLOB:

```Java
Statement statement = connection.createStatement();
    ResultSet results = statement.executeQuery("SELECT * FROM user");
    results.first();

    Blob blob = results.getBlob(3);
    InputStream is = blob.getBinaryStream();
```

Чтобы создать свой объект Blob, нужно воспользоваться функцией createBlob(). Пример:

```Java
String insertQuery = “INSERT INTO images(name, image) VALUES (?, ?)”;
PreparedStatement statement = connection.prepareStatement(insertQuery);

// Создаем объект Blob и получаем у него OtputStream для записи в него данных
Blob blob = connection.createBlob();

// Заполняем Blob данными  …
OutputStream os = blob.setBinaryStream(1);

// Передаем Вlob как параметр запроса
statement.setBlob(2, blob);
statement.execute();
```

Заполнить Blob данными можно двумя способами. Первый — через OutputSteam:

```Java
Path avatar = Paths.get("E:\\images\\cat.jpg");
OutputStream os = blob.setBinaryStream(1);
Files.copy(avatar, os);
```

И второй — через заполнение байтами:

```Java
Path avatar = Paths.get("E:\\images\\cat.jpg");
byte[] content = Files.readAllBytes(avatar);
blob.setBytes(1, content);
```


------

### bytea

Цель следующая:

1. Превратить экземлпяр класса Person в набор байтов, записать в таблицу байты
2. Считать байты, десериализовать в Person

```Java
import java.io.Serializable;

public class Person implements Serializable {
    private String name;
    private String lastName;
    private int salary;
    private int department_id;

    public Person(String name, String lastName, int salary, int department_id) {
        this.name = name;
        this.lastName = lastName;
        this.salary = salary;
        this.department_id = department_id;
    }

    @Override
    public String toString() {
        return "Person{" +
               "name='" + name + '\'' +
               ", lastName='" + lastName + '\'' +
               ", salary=" + salary +
               ", department_id=" + department_id +
               '}';
    }
}
```

```Java
Person person = new Person("Vadim", "Konovalov", 1000, 1);
String insert = "INSERT INTO blob_employees (object) VALUES(?)";
insert(person, insert);
```

```Java
private static void insert(Person person, String insert) throws SQLException, IOException {
        try (var connection = ConnectionManager.open();
             var statement = connection.prepareStatement(insert);
             ByteArrayOutputStream bos = new ByteArrayOutputStream();
             ObjectOutputStream oos = new ObjectOutputStream(bos)) {
            oos.writeObject(person);
            oos.flush();
            statement.setBytes(1, bos.toByteArray());
            statement.execute();
        }
    }
```

Результат выполнения данного кода

![[images/Untitled 2 12.png|Untitled 2 12.png]]

Десериализация

```Java
String select = "SELECT * FROM blob_employees WHERE id = 1";
Person gettedPerson = getPerson(select);
System.out.println(gettedPerson);
```

```Java
private static Person getPerson(String select) throws SQLException, IOException, ClassNotFoundException {
        try (var connection = ConnectionManager.open();
             var statement = connection.createStatement()){
            ResultSet resultSet = statement.executeQuery(select);
            resultSet.next();
            byte[] bytes = resultSet.getBytes("object");
            ByteArrayInputStream bis = new ByteArrayInputStream(bytes);
            ObjectInputStream ois = new ObjectInputStream(bis);
            return (Person) ois.readObject();
        }
    }
```

```Java
ВЫВОД:Person{name='Vadim', lastName='Konovalov', salary=1000, department_id=1}
```

  

  

---
title: Connection Jdbc
tags:
  - jdbc
related_topics: 
created: 2025-04-25 17:32
modified: 2025-04-29T13:27:43+03:00
questions: 
notes: 
links: 
---


<mark class="hltr-red">Connection это интерфейс</mark><mark class="hltr-yellow"> extends Wrapper, AutoCloseable (необходимо закрывать либо оборачивать в try with resources)</mark>


![[Pasted image 20250425173547.png]]

Подключиться к БД можно следующим образом:


```Java
String url = "jdbc:postgresql://localhost:5432/postgres";
String userName = "vadimko99";
String password = "knopka9955";
try{
Connection connection = DriverManager.getConnection(url, userName, password);
}
catch(Exception e){}
finally{
connection.close();
}
```

Но по-хорошему стоит создать отдельный утилитарный класс следующим образом:

```Java
package org.example;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public final class ConnectionManager {
    private static final String URL = "jdbc:postgresql://localhost:5432/postgres";
    private static final String USER_NAME = "vadimko99";
    private static final String PASSWORD = "knopka9955";

    static {
        loadDriver();
    }

    private static void loadDriver() {
        try {
            Class.forName("org.postgresql.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

    private ConnectionManager(){
    }

    public static Connection open() throws RuntimeException {
        try{
            return DriverManager.getConnection(URL, USER_NAME, PASSWORD);
        } catch (SQLException e){
            throw new RuntimeException(e);
        }

    }
}
```

```Java
package org.example;

import java.sql.Connection;

public class JDBCRunner {
    public static void main(String[] args) {
		try(Connection connection = ConnectionManager.open()){
}

    }
}
```

# <mark class="hltr-green2">Еще лучше вынести все в resources/application.properties и вытаскивать значения оттуда с помощью PropertiesUtil</mark>

```Java
// application.properties

db.url=jdbc:postgresql://localhost:5432/postgres
db.username=vadimko99
db.password=knopka9955
```

```Java
import java.sql.Connection;

public class JDBCRunner {
    public static void main(String[] args) {
try(Connection connection = ConnectionManager.open()){
       }
    }
}
```

```Java
import example.util.PropertiesUtil;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public final class ConnectionManager {
    private static final String URL_KEY = "db.url";
    private static final String USER_NAME_KEY = "db.username";
    private static final String PASSWORD_KEY ="db.password";

    static {
        loadDriver();
    }

    private static void loadDriver() {
        try {
            Class.forName("org.postgresql.Driver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

    private ConnectionManager(){
    }

    public static Connection open() throws RuntimeException {
        try{
            return DriverManager.getConnection(
                    PropertiesUtil.get(URL_KEY),
                    PropertiesUtil.get(USER_NAME_KEY),
                    PropertiesUtil.get(PASSWORD_KEY));
        } catch (SQLException e){
            throw new RuntimeException(e);
        }

    }
}
```

```Java
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class PropertiesUtil {
    private static final Properties PROPERTIES = new Properties();

    static {
        loadProperties();
    }
    public static String get(String key){
        return PROPERTIES.getProperty(key);
    }

    private static void loadProperties() {
        try (InputStream stream = PropertiesUtil.class.getClassLoader().getResourceAsStream("application.properties")) {
            PROPERTIES.load(stream);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private PropertiesUtil(){}

}
```

  -----

![[Pasted image 20250429132733.png]]
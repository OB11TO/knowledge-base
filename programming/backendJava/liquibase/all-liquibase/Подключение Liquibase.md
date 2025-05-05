---
title: Подключение Liquibase
tags:
  - Liquibase
related_topics: 
created: 2025-04-30 14:55
modified: 2025-04-30T15:48:15+03:00
questions: 
notes: 
links: 
---


---

## Maven

- <mark class="hltr-orange">liquibase-maven-plugin</mark>

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.liquibase</groupId>
      <artifactId>liquibase-maven-plugin</artifactId>
      <version>4.31.0</version>
      <configuration>
        <!-- Можно указать файл liquibase.properties -->
        <propertyFile>src/main/resources/liquibase.properties</propertyFile>
      </configuration>
      <executions>
        <execution>
          <phase>process-resources</phase>
          <goals><goal>update</goal></goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>

```

- <mark class="hltr-purple">liquibase.properties</mark>

```java
url=jdbc:postgresql://localhost:5432/mydb
username=${env.DB_USER}
password=${env.DB_PASS}
driver=org.postgresql.Driver
changeLogFile=src/main/resources/db/changelog/db.changelog-master.sql

```


-----
## Gradle

```java
plugins {
    id 'org.liquibase.gradle' version '2.0.4'
}
liquibase {
    activities {
        main {
            changeLogFile 'src/main/resources/db/changelog/db.changelog-master.yaml'
            url 'jdbc:postgresql://localhost:5432/myapp'
            username 'user'
            password 'pass'
        }
    }
    runList = 'main'
}
```




---

## Spring Boot

```java
<dependency>
  <groupId>org.liquibase</groupId>
  <artifactId>liquibase-core</artifactId>
  <version>4.31.1</version>
</dependency>

OR

implementation 'org.liquibase:liquibase-core:4.31.1'

```
  

![[Pasted image 20250430154803.png]]


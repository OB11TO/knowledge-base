---
title: Подключение библиотеки AssertJ 504881
tags:
  - Testing
related_topics: 
created: 2025-01-16 13:18
modified: 2025-01-16T13:24:11+03:00
questions: 
notes: 
links: 
---

_**Модульное тестирование** представляет собой процесс тестирования результатов работы отдельных методов, из которых состоит ваша программа. Модульное тестирование - это первый уровень борьбы с багами - то есть с ошибками в программировании.

В курсе в качестве среды тестирования используются библиотеки **JUnit 5** и **AssertJ**. Давайте подключим их к нашему проекту. Откройте файл **pom.xml** и в блок зависимостей **<dependencies>...</dependencies>** добавьте зависимости (они уже должны быть подключены на предыдущих уроках) :

```xml
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.10.2</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.assertj</groupId>
            <artifactId>assertj-core</artifactId>
            <version>3.26.0</version>
            <scope>test</scope>
        </dependency>

```

Для проверки тестов на сервере **job4j**, надо добавить плагин **maven-surefire-plugin**. Для этого в файле **pom.xml** в блок **<plugins>...</plugins>** добавьте строки (он тоже уже должен быть)

```xml
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>3.3.0</version>
          <configuration>
            <skipTests>false</skipTests>
          </configuration>
        </plugin>

```


----

![[Pasted image 20250116132355.png]]

![[Pasted image 20250116132411.png]]

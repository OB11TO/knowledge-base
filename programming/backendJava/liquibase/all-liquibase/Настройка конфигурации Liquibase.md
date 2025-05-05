---
title: Настройка конфигурации Liquibase
tags:
  - Liquibase
related_topics: 
created: 2025-04-30 15:39
modified: 2025-04-30T15:40:40+03:00
questions: 
notes: 
links: 
---

**Файл `liquibase.properties`** (в корне проекта или classpath):
```java
url=jdbc:postgresql://localhost:5432/mydb
username=admin
password=secret
changeLogFile=db/changelog/changelog-master.xml
driver=org.postgresql.Driver
```

```java
liquibase --url="jdbc:postgresql://localhost/mydb" --changeLogFile=db/changelog.xml update
```

-----


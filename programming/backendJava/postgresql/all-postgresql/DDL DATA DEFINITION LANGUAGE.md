---
title: DDL DATA DEFINITION LANGUAGE
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:09
modified: 2024-09-24T15:10:06+03:00
questions: 
notes: 
links: 
---

### DDL (DATA DEFINITION LANGUAGE)

- CREATE создает таблицу или другой объект в БД
- ALTER модиифицирует существующий в БД объект, такой как таблица
- DROP удаляет объект в БД

```SQL
-- CREATE 
CREATE DATABASE repository;
CREATE SCHEMA ...
CREATe TABLE employees;


--ALTER 

ALTER TABLE employees someAction();
ALTER TABLE employees ADD column email VARCHAR(20);

--DROP удаляет существующую таблицу, представление таблицы или другой объект в БД
DROP TABLE employees;
```

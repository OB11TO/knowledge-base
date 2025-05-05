---
title: DML DATA MANIPULATION LANGUAGE
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:10
modified: 2024-09-24T15:12:51+03:00
questions: 
notes: 
links: 
---

### DML (DATA MANIPULATION LANGUAGE)

- SELECT извлекает записи
- INSERT создает записи
- UPDATE модифицирует записи
- DELETE удаляет записи
- TRUNCATE очищает все записи из таблицы

```SQL
--SELECT
SELECT * from employees where salary > 1000;

--INSERT

INSERT INTO employees (id,name, lasst_name,salary)
VALUES (1, 'Vadim', 'Konovalov', 1000);

--UPDATE
UPDATE PERSON SET name = 'Dmitriy' where last_name = 'Konovalov'

--DELETE

DELETE from employees where last_name LIKE '%lov';
```

  
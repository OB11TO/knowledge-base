---
title: CONSTRAINTS
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:12
modified: 2024-09-24T15:14:15+03:00
questions: 
notes: 
links: 
---

### CONSTRAINTS

- UNIQUE уникальный
- NOT NULL
- CHECK проверка условия
- PRIMARY KEY = UNIQUE + NOT NULL первичный ключ
- FOREIGN KEY связывание с внешней бд
- SERIAL автоинкремент  


```sql
CREATE TABLE employees (
id SERIAL PRIMARY KEY,

name VARCHAR(128) NOT NULL,

dateOFWork DATE VARCHAR(32) CHECK(dateOfWork BETWEEN '01-01-2000' AND '12-12-2023'),
pc_no INT UNIQUE,
company_id INT REFERENCES companys(id)  -- FOREIGN KEY
);
```

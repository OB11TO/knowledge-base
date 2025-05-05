---
title: sql запросы для логического устройства
tags:
  - PostgreSql
related_topics: 
created: 2024-10-10 15:08
modified: 2024-10-10T15:08:33+03:00
questions: 
notes: 
links: 
---


-- database

-- system catalog

```
SELECT oid, datname, datistemplate, datallowconn FROM pg_database;
```

-- size

```
SELECT pg_size_pretty(pg_database_size('postgres'));
```

-- schema

```
\dn
```

-- current schema

```
SELECT current_schema();
```

-- view table

```
\d pg_database

select * from pg_database;
```

-- seach path

```
SHOW search_path;
```

 -- SET search_path to .. - в рамках сессии

-- параметр можно установить и на уровне отдельной базы данных:

-- ALTER DATABASE otus SET search_path = public, special;

-- в рамках кластера в файле postgresql.conf

\dt

-- интересное поведение и search_path

```
\d pg_database

CREATE TABLE pg_database(i int);
```

-- все равно видим pg_catalog.pg_database

```
\d pg_database
```

-- чтобы получить доступ к толко что созданной таблице используем указание схемы

```
\d public.pg_database

SELECT * FROM pg_database limit 1;
```

-- в 1 схеме или разных?

```
CREATE TABLE t1(i int);

CREATE SCHEMA postgres;

CREATE TABLE t2(i int);

CREATE TABLE t1(i int);

\dt

\dt+

\dt public.*

SET search_path TO public, "$user";

\dt

SET search_path TO public, "$user", pg_catalog;

\dt

create temp table t1(i int);

\dt

SET search_path TO public, "$user", pg_catalog, pg_temp;

\dt
```

-- можем переносить таблицу между схемами - при этом меняется только запись в pg_class, физически данные на месте

```
ALTER TABLE t2 SET SCHEMA public;
```

-- чтобы не было вопросов указываем схему прямо при создании

```
CREATE TABLE public.t10(i int);
```

-- relations

```
SELECT relkind, count(*) FROM pg_class GROUP BY relkind;
relkind | count 
---------+-------
 таблицы
 r       |    69
 представления
 v       |   137
 индексы
 i       |   158
 тост сегменты
 t       |    40
 последовательности
 S       |     1
```
---
title: sql буферный кеш
tags:
  - PostgreSql
related_topics: 
created: 2024-10-11 18:33
modified: 2024-10-11T18:35:17+03:00
questions: 
notes: 
links: 
---



-- проверим размер кеша

```
SELECT setting, unit FROM pg_settings WHERE name = 'shared_buffers';
```

-- уменьшим количество буферов для наблюдения

```
ALTER SYSTEM SET shared_buffers = 200;
```

-- рестартуем кластер после изменений

```
pg_ctlcluster 14 main restart

CREATE DATABASE buffer_temp;

\c buffer_temp

CREATE TABLE test(i int);
```

-- сгенерируем значения

```
INSERT INTO test SELECT s.id FROM generate_series(1,100) AS s(id); 

SELECT * FROM test limit 10;
```

-- создадим расширение для просмотра кеша

```
CREATE EXTENSION pg_buffercache; 

\dx+
```

-- создадим представление для просмотра содержимого буферного кеша

```
CREATE VIEW pg_buffercache_v AS

SELECT bufferid,

       (SELECT c.relname FROM pg_class c WHERE  pg_relation_filenode(c.oid) = b.relfilenode ) relname,

       CASE relforknumber

         WHEN 0 THEN 'main'

         WHEN 1 THEN 'fsm'

         WHEN 2 THEN 'vm'

       END relfork,

       relblocknumber,

       isdirty,

       usagecount

FROM   pg_buffercache b

WHERE  b.relDATABASE IN (    0, (SELECT oid FROM pg_DATABASE WHERE datname = current_database()) )

AND    b.usagecount is not null;

SELECT * FROM pg_buffercache_v WHERE relname='test';

SELECT * FROM test limit 10;

UPDATE test set i = 2 WHERE i = 1;
```

-- увидим грязную страницу

```
SELECT * FROM pg_buffercache_v WHERE relname='test';
```

-- даст пищу для размышлений над использованием кеша -- usagecount > 3

```
SELECT c.relname,

  count(*) blocks,

  round( 100.0 * 8192 * count(*) / pg_TABLE_size(c.oid) ) "% of rel",

  round( 100.0 * 8192 * count(*) FILTER (WHERE b.usagecount > 3) / pg_TABLE_size(c.oid) ) "% hot"

FROM pg_buffercache b

  JOIN pg_class c ON pg_relation_filenode(c.oid) = b.relfilenode

WHERE  b.relDATABASE IN (

         0, (SELECT oid FROM pg_DATABASE WHERE datname = current_database())

       )

AND    b.usagecount is not null

GROUP BY c.relname, c.oid

ORDER BY 2 DESC

LIMIT 10;
```

-- сгенерируем значения с текстовыми полями - чтобы занять больше страниц

```
CREATE TABLE test_text(t text);

INSERT INTO test_text SELECT 'строка '||s.id FROM generate_series(1,500) AS s(id); 

SELECT * FROM test_text limit 10;

SELECT * FROM test_text;

SELECT * FROM pg_buffercache_v WHERE relname='test_text';
```

-- посмотрим на прогрев кеша

-- рестартуем кластер для очистки буферного кеша

```
sudo pg_ctlcluster 14 main restart

\c buffer_temp

SELECT * FROM pg_buffercache_v WHERE relname='test_text';

CREATE EXTENSION pg_prewarm;

SELECT pg_prewarm('test_text');

SELECT * FROM pg_buffercache_v WHERE relname='test_text';
```
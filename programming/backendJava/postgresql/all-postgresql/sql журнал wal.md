---
title: sql журнал wal
tags:
  - PostgreSql
related_topics: 
created: 2024-10-11 19:42
modified: 2024-10-11T19:43:03+03:00
questions: 
notes: 
links: 
---


-------------WAL-----------------

```
\c buffer_temp

SELECT * FROM pg_ls_waldir() LIMIT 10;

CREATE EXTENSION pageinspect;

BEGIN;
```

-- текущая позиция lsn

```
SELECT pg_current_wal_insert_lsn();
```

-- посмотрим какой у нас wal file

```
SELECT pg_walfile_name('0/1670928');
```

-- после UPDATE номер lsn изменился

```
SELECT lsn FROM page_header(get_raw_page('test_text',0));

commit;

UPDATE test_text set t = '1' WHERE t = 'строка 1';

SELECT pg_current_wal_insert_lsn();

SELECT lsn FROM page_header(get_raw_page('test_text',0));

SELECT '0/1672DB8'::pg_lsn - '0/1670928'::pg_lsn;

/usr/lib/postgresql/14/bin/pg_waldump -p /var/lib/postgresql/14/main/pg_wal -s 0/1670928 -e 0/1672DB8 000000010000000000000001
```
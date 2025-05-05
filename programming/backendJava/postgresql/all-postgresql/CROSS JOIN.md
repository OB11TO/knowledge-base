---
title: CROSS JOIN
tags:
  - PostgreSql
related_topics: 
created: 2024-09-25 15:35
modified: 2024-09-25T15:35:45+03:00
questions: 
notes: 
links: 
---

<mark class="hltr-yellow">Декартово произведение</mark>, например перемножить места в таблице seat для самолета


```SQL
INSERT INTO seat(aircraft_id, seat_no)
SELECT id, s.column1 from aircraft
CROSS JOIN (VALUES ('A1'),('A2'),('B1'),('B2'),('C1'),('C2'),('D1'),('D2') order by 1);
```

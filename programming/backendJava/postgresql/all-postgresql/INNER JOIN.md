---
title: INNER JOIN
tags:
  - PostgreSql
related_topics: 
created: 2024-09-25 15:32
modified: 2024-12-02T16:37:56+03:00
questions: 
notes: 
links: 
---

### INNER JOIN

или просто JOIN . объединяет,<mark class="hltr-yellow"> не беря в расчет данные, у которых нет связи</mark>

```SQL
SELECT * from employees JOIN companys ON employees.id = companys.id;
```


![[Pasted image 20241202163745.png]]

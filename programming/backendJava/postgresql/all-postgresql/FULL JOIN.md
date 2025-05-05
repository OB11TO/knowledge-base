---
title: FULL JOIN
tags:
  - PostgreSql
related_topics: 
created: 2024-09-25 15:35
modified: 2024-09-25T15:36:07+03:00
questions: 
notes: 
links: 
---


```SQL
SELECT customers.customer_name, orders.order_date
FROM customers
FULL JOIN orders ON customers.customer_id = orders.customer_id;
```

Результатом этого запроса будет<mark class="hltr-yellow"> таблица, содержащая все имена клиентов и даты заказов, при этом значения будут</mark> `NULL`, если для них нет соответствующей записи в другой таблице. Таким образом, результат будет таким:

|   |   |
|---|---|
|customer_name|order_date|
|John|2022-04-15|
|John|2022-04-17|
|Emily|2022-04-16|
|Michael|2022-04-18|
|Sarah|NULL|
|David|NULL|
|NULL|2022-04-19|

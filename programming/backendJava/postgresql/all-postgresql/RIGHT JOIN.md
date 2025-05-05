---
title: RIGHT JOIN
tags:
  - PostgreSql
related_topics: 
created: 2024-09-25 15:34
modified: 2024-09-25T15:35:01+03:00
questions: 
notes: 
links: 
---


В запросе ниже мы используем `RIGHT JOIN`, чтобы выбрать все записи из таблицы `orders`, а также связанные с ней записи из таблицы `customers`, если они существуют:

```SQL
SELECT customers.customer_name, orders.order_date
FROM customers
RIGHT JOIN orders ON customers.customer_id = orders.customer_id;
```

Результатом этого запроса будет таблица, в которой будут перечислены все даты заказов из таблицы `orders`, а также имена клиентов, если они есть. Если для заказа нет соответствующего клиента, то значение в столбце `customer_name` будет `NULL`. Таким образом, результат будет таким:

|   |   |
|---|---|
|customer_name|order_date|
|John|2022-04-15|
|Emily|2022-04-16|
|John|2022-04-17|
|Michael|2022-04-18|
|NULL|2022-04-19|

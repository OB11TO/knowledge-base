---
title: LEFT JOIN
tags:
  - PostgreSql
related_topics: 
created: 2024-09-25 15:33
modified: 2024-12-02T16:38:31+03:00
questions: 
notes: 
links: 
---

Таблица `customers`:

|             |               |
| ----------- | ------------- |
| customer_id | customer_name |
| 1           | John          |
| 2           | Emily         |
| 3           | Michael       |
| 4           | Sarah         |
| 5           | David         |

Таблица `orders`:

|   |   |   |
|---|---|---|
|order_id|order_date|customer_id|
|1|2022-04-15|1|
|2|2022-04-16|2|
|3|2022-04-17|1|
|4|2022-04-18|3|
|5|2022-04-19|1|

1. `LEFT JOIN`

В запросе ниже мы используем `LEFT JOIN`, чтобы выбрать все записи из таблицы `customers`, а также связанные с ней записи из таблицы `orders`, если они существуют


```SQL
SELECT customers.customer_name, orders.order_date
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id;

```

Результатом этого запроса будет таблица, в которой будут перечислены все имена клиентов из таблицы `customers`, а также даты заказов, если они есть. Если для клиента нет заказов, то значение в столбце `order_date` будет `NULL`. Таким образом, результат будет таким:

|   |   |
|---|---|
|customer_name|order_date|
|John|2022-04-15|
|John|2022-04-17|
|Emily|2022-04-16|
|Michael|2022-04-18|
|Sarah|NULL|
|David|NULL|

![[Pasted image 20241202163816.png]]


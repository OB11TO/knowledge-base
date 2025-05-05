---
title: FOREIGN KEYS виды
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:39
modified: 2024-09-25T14:00:05+03:00
questions: 
notes: 
links: 
---


----
[[One to Many]]
[[One to One]]
[[Many to One]]
[[Many to Many]]

[[Операции и поведение внешних ключей]]
[[Foreign Keys не на первичный ключ]]

----


### FOREIGN KEYS виды (Связи между таблицами)

### Виды и нюансы **Foreign Key** (Внешних ключей) в реляционных базах данных

Внешний ключ (**Foreign Key**) — это колонка или набор колонок, которые создают ссылку на уникальное поле в другой таблице (чаще всего это первичный ключ). Внешний ключ используется для обеспечения целостности данных и создания связей между таблицами.

### Основные виды внешних ключей:

 **Простой внешний ключ**:
- Внешний ключ, состоящий из одного столбца, который ссылается на уникальный столбец (чаще всего первичный ключ) другой таблицы.
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

```

В этом примере столбец `customer_id` является внешним ключом и ссылается на столбец `id` таблицы `customers`.

**Составной (композитный) внешний ключ**:
- Внешний ключ, состоящий из нескольких колонок, который ссылается на составной первичный ключ другой таблицы.
```sql
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

```
В данном примере `order_items` содержит два внешних ключа: один на таблицу `orders` через поле `order_id` и другой на таблицу `products` через поле `product_id`.

**Самоссылающийся внешний ключ**:
- Внешний ключ в таблице, который ссылается на другой столбец той же таблицы. Это используется для создания иерархий, например, для обозначения "родитель-потомок" в одной таблице.
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    manager_id INT,
    FOREIGN KEY (manager_id) REFERENCES employees(id)
);

```
В этом случае столбец `manager_id` ссылается на `id` в той же таблице, чтобы отразить структуру "сотрудник-менеджер".
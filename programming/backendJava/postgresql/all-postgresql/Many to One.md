---
title: Many to One
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:55
modified: 2024-09-24T16:00:02+03:00
difficulty: 
questions: 
notes: 
links: 
---

Связь **"многие к одному" (Many-to-One)** означает, что многие записи одной таблицы могут быть связаны с одной записью другой таблицы. Пример такой связи может быть таблица `Order` и таблица `Customer`, где один заказ (`Order`) относится к одному клиенту (`Customer`), но один клиент может иметь много заказов.

### Пример таблиц для связи "многие к одному"

1. **Таблица `Customer`**:

|id|name|
|---|---|
|1|John Doe|
|2|Jane Doe|

2. **Таблица `Order`** (каждый заказ связан с клиентом через внешний ключ `customer_id`):

|id|order_date|customer_id|
|---|---|---|
|1|2024-09-01|1|
|2|2024-09-02|1|
|3|2024-09-03|2|

В этом примере клиент John Doe имеет два заказа, а клиент Jane Doe — один заказ.

### Пример на Spring Data JPA

1. **Сущность `Customer` (Клиент)**
```java
import jakarta.persistence.*;
import java.util.List;

@Entity
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Двунаправленная связь One-to-Many с таблицей Order
    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders;

```

2. Сущность `Order` (Заказ)
```java
import jakarta.persistence.*;
import java.time.LocalDate;

@Entity
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private LocalDate orderDate;

    // Связь Many-to-One с таблицей Customer
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "customer_id", nullable = false)
    private Customer customer;

```

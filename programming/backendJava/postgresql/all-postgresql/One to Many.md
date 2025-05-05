---
title: One to Many
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:48
modified: 2024-09-24T15:48:43+03:00
questions: 
notes: 
links: 
---

### `ONE to MANY` Покупатель -Заказы, РК -уникальный, FK - может иметь дубликаты

```SQL
CREATE TABLE CHILD(
child_id INT,
parent_id NOT NULL REFERENCES parents(parent_id)
);
```

![[images/Untitled 15 4.png|Untitled 15 4.png]]

```java
import jakarta.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String position;

    // Двунаправленная связь One-to-Many с таблицей Contact
    @OneToMany(mappedBy = "employee", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Contact> contacts = new ArrayList<>();

```


```java
import jakarta.persistence.*;

@Entity
public class Contact {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String contactType;
    private String contactValue;

    // Двунаправленная связь Many-to-One с таблицей Employee
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id", nullable = false)
    private Employee employee;

```


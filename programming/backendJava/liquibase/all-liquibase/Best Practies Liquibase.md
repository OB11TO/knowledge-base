---
title: Best Practies Liquibase
tags:
  - Liquibase
related_topics: 
created: 2025-04-30 15:30
modified: 2025-04-30T15:32:06+03:00
questions: 
notes: 
links: 
---


```sql
src/main/resources/db/changelog/
  ├── db.changelog-master.xml        // Главный файл
  ├── v1.0/                       // Версия или функциональный блок
  │   ├── create-tables.xml
  │   └── initial-data.xml
  └── v1.1/
      └── alter-tables.xml
```


![[Pasted image 20250430153155.png]]


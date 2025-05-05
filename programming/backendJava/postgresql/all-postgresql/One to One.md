---
title: One to One
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:49
modified: 2024-09-24T15:50:50+03:00
questions: 
notes: 
links: 
---



### `ONE to ONE` Гражданин - Паспорт , не может быть id, которого нет в паспортах

```SQL
CREATE TABLE passport(
passport_id INT PRIMARY KEY REFERENCES citizens(citizen_id)
);
```

![[images/Untitled 16 4.png|Untitled 16 4.png]]

![[images/Untitled 17 4.png|Untitled 17 4.png]]

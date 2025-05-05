---
title: Many to Many
tags:
  - PostgreSql
related_topics: 
created: 2024-09-24 15:51
modified: 2024-09-24T15:53:14+03:00
questions: 
notes: 
links: 
---



### `MANY to MANY` Актеры - Фильмы, создается join таблица

```SQL
CREATE TABLE actors_movies(
actor_id INT REFERENCES actors(actor_id),

movie_id INT REFERENCES citizens(movie_id)
);
```

![[images/Untitled 18 3.png|Untitled 18 3.png]]

---
title: Task graph
tags:
  - Gradle
related_topics: 
created: 2024-09-16 18:31
modified: 2024-09-18T15:43:20+03:00
questions: 
notes: 
links: 
---


## Task graph

![[images/Untitled 21 4.png|Untitled 21 4.png]]


#### Зависимость Task

- ==Можно прописывать зависимости== `task` с ==помощью== `dependsOn(Objects…. paths)`

![[images/Untitled 37 4.png|Untitled 37 4.png]]

![[images/Untitled 38 4.png|Untitled 38 4.png]]

- Есть `finalizedBy` которая говорит, что ==сначала 1, потом 2==

![[images/Untitled 39 4.png|Untitled 39 4.png]]

- `mustRunAfter` - ==обязан== идти после second
- `shouldRunAfter` - ==должен== идти после second

![[images/Untitled 40 4.png|Untitled 40 4.png]]

  

![[images/Untitled 41 4.png|Untitled 41 4.png]]

![[images/Untitled 42 4.png|Untitled 42 4.png]]

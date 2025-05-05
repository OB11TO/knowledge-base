---
title: Транзитивные зависимости Gradle
tags:
  - Gradle
related_topics: 
created: 2024-09-16 18:34
modified: 2024-09-24T12:19:53+03:00
questions: 
notes: 
links: 
---

## Транзитивные зависимости

![[images/Untitled 64 2.png|Untitled 64 2.png]]

В `Gradle` е<mark class="hltr-green2">сть дефолтная стратегия, которая берет версию выше всегда</mark>



- Также можно делать `force(true)` - мы<mark class="hltr-yellow"> используем эту зависимость по умолчанию</mark>, а остальные (даже <mark class="hltr-red">старшие версии не подтягиваем</mark>)
![[Pasted image 20240923114756.png]]

![[images/Untitled 65 2.png|Untitled 65 2.png]]
- Или можем так
![[Pasted image 20240923115038.png]]

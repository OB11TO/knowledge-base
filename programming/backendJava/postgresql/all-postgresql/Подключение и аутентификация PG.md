---
title: Подключение и аутентификация PG
tags:
  - PostgreSql
related_topics: 
created: 2024-09-25 15:38
modified: 2025-04-09T13:27:50+03:00
questions: 
notes: 
links: 
---
 
![[Pasted image 20240924180140.png]]

- <mark class="hltr-orange">Подключение через Unix Socket </mark>
![[Pasted image 20240924180448.png]]

- <mark class="hltr-orange">Подключение через localhost</mark>, но нужно конкретно это указать<mark class="hltr-red"> -h</mark> <\локалхост> <mark class="hltr-red">-U </mark><\пользователь><mark class="hltr-red"> -d</mark> <\база данных> 
![[Pasted image 20240924181452.png]]
---
title: Sql Injection
tags:
  - jdbc
related_topics: 
created: 2025-04-28 13:43
modified: 2025-04-28T13:59:55+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20250428135405.png]]
- <mark class="hltr-red">Это плохой вариант !!!!!</mark>
- Потому что есть<mark class="hltr-yellow"> инъекция для строки, которая может быть заменена и стать шансом хакерских атак!</mark>
- Это очень плохо и не безопасно. <mark class="hltr-red">ПОЭТОМУ Statment редко используют.</mark>
![[Pasted image 20250428135605.png]]

 ![[Pasted image 20250428135906.png]]

- <mark class="hltr-green2">Нужно использовать PrepeaStetment!!</mark>


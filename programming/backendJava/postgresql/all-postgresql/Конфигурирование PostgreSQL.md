---
title: Конфигурирование PostgreSQL
tags:
  - PostgreSql
related_topics: 
created: 2024-09-25 17:02
modified: 2024-09-27T11:29:38+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20240925174557.png]]![[Pasted image 20240925174842.png]]
 ![[Pasted image 20240925174910.png]]
 ![[Pasted image 20240925174933.png]]

Конечно далеко не все из них активно используются.

Можно разделить их  по контексту (полю context в запросе)

**select name, context from pg_settings;**

[https://postgrespro.ru/docs/postgresql/14/view-pg-settings](https://postgrespro.ru/docs/postgresql/14/view-pg-settings)

- самые простые на сеанс имеют контекст user
- для сессии уже повыше уровень - backend и требуются соответствующие права
- есть уровень superuser - для суперпользователя
- остальные настройки уже более глобальные и обычно требуют перезапуска сервера

<mark class="hltr-red">2 онлайн конфигуратора. Просто указывайте параметры вашего сервера и профиль нагрузки и вам предложат оптимальные настройки:</mark>

[https://pgtune.leopard.in.ua/#/](https://pgtune.leopard.in.ua/)

[http://pgconfigurator.cybertec.at/](http://pgconfigurator.cybertec.at/)
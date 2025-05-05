---
title: HTTPClientExample
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:56
modified: 2024-11-07T13:10:55+03:00
questions: 
notes: 
links: 
---

### HTTPClientExample

Появился с версии Java11 и выступает в роли клиента к серверу. Стал предпочтительный для использования по сравнению с URL. Имеет более удобный API и расширенный функционал.

Можно либо создать дефолтный:

```Java
HttpClient.newHttpClient();
```

Либо создать через паттерн builder настривая различные параметры.

![[Untitled 44.png]]

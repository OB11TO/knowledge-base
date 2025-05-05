---
title: Interfaces
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 17:38
modified: 2025-03-20T19:22:46+03:00
questions: 
notes: 
links: 
---

### Interfaces

- `Closeable`: Интерфейс `Closeable` <mark class="hltr-yellow">определяет метод</mark> `close()`, который <mark class="hltr-purple">должен быть реализован классами, чтобы освободить ресурсы, связанные с объектом.</mark>

- `Flushable`: Интерфейс `Flushable` <mark class="hltr-yellow">определяет метод</mark> `flush()` который <mark class="hltr-purple">должен быть реализован классами, чтобы сбросить буферизованные данные в выходной поток.</mark>

- `Serializable`: Интерфейс `Serializable` используется для указания, что <mark class="hltr-yellow">класс может быть сериализован и десериализован.</mark>

- `AutoCloseable`: Интерфейс `AutoCloseable` <mark class="hltr-yellow">расширяет</mark> `Closeable` <mark class="hltr-yellow">и определяет метод</mark> `close()`, который <mark class="hltr-blue">может генерировать исключения.</mark>

- `DataInput`: Интерфейс `DataInput` <mark class="hltr-blue">определяет методы для чтения примитивных данных из входного потока.</mark>

- `DataOutput`: Интерфейс `DataOutput` <mark class="hltr-blue">определяет методы для записи примитивных данных в выходной поток.</mark>

- `ObjectInput`: Интерфейс `ObjectInput` <mark class="hltr-red">предоставляет методы для чтения объектов из входного потока.</mark>

- `ObjectOutput`: Интерфейс `ObjectOutput` <mark class="hltr-red">предоставляет методы для записи объектов в выходной поток.</mark>

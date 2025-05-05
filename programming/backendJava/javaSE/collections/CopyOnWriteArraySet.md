---
title: CopyOnWriteArraySet
tags:
  - Collection
  - Set
  - Concurrent
related_topics: 
created: 2024-09-09 17:21
modified: 2024-09-09T17:24:15+03:00
questions: 
notes: 
links: 
---
### class <mark class="hltr-orange">CopyOnWriteArraySet</mark><\E> (java.util.concurrent)

![[Pasted image 20240909172258.png]]

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)\<E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)\<E>, [Set](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)<E

является потокобезопасной реализацией интерфейса `Set<E>` в Java, которая обеспечивает консистентность данных при одновременном доступе из нескольких потоков.

Плюсы:

1. Потокобезопасность: `CopyOnWriteArraySet<E>` обеспечивает потокобезопасность без необходимости использования дополнительных механизмов синхронизации, таких как явная синхронизация или блокировки.
2. Итератор безопасен: Итераторы, возвращаемые `CopyOnWriteArraySet<E>`, не выбрасывают `ConcurrentModificationException`, так как они работают на неизменной копии исходных данных.
3. Высокая производительность для чтения: При чтении данных нет блокировок и синхронизации, что может привести к улучшенной производительности в случае, когда чтение является основной операцией.

Минусы:

1. Потребление памяти: Каждое изменение `CopyOnWriteArraySet<E>` <mark class="hltr-yellow">приводит к созданию полной копии внутреннего массива, что может привести к значительному потреблению памяти</mark> при большом объеме данных или частых изменениях.
2. Задержка обновлений: Изменения в `CopyOnWriteArraySet<E>` <mark class="hltr-yellow">не отражаются немедленно, а создаются копия данных. Поэтому, если другой поток выполняет чтение в то время, когда изменения еще не применены, он может получить устаревшие данные.</mark>
3. Не подходит для частых изменений: `CopyOnWriteArraySet<E>` <mark class="hltr-yellow">предназначен в основном для ситуаций, когда изменения происходят редко по сравнению с операциями чтения.</mark> Если изменения происходят часто, то `CopyOnWriteArraySet<E>` может оказаться неэффективным в плане производительности.
---
title: Interface Collection
tags:
  - Collection
related_topics:
  - "[[Interface List]]"
  - "[[Interface Set]]"
  - "[[Interface Queue]]"
created: 2024-09-09 15:35
modified: 2024-09-12T15:59:42+03:00
difficulty: 
questions: 
notes: 
links: 
---
# Interface Collection

[https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)

![[images/Untitled 11.png|Untitled 11.png]]

Родитель интерфейс - Iterable\<E>

Не работают с примитивными типами, только с классами обертками.

<mark class="hltr-yellow">Для правильного поиска объектов в коллекциях необходимо переопределять equals и hashcode.
</mark>
<mark class="hltr-green2">Для правильной сортировки объектов необходимо, чтобы класс имплементировал Comparable, вызывая метод сортировки, можно передавать в аргументы Comparator</mark>

  

All Known Subinterfaces:
[BeanContext](https://docs.oracle.com/javase/8/docs/api/java/beans/beancontext/BeanContext.html)
[BeanContextServices](https://docs.oracle.com/javase/8/docs/api/java/beans/beancontext/BeanContextServices.html), 
[BlockingDeque](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingDeque.html)\<E>, [BlockingQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html)\<E>, [Deque](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html\)\<E>, [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)\<E>, [NavigableSet](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html)\<E>, [Queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)\<E>, [Set](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)\<E>, [SortedSet](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)\<E>, [TransferQueue](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/TransferQueue.html)\<E>

![[images/Untitled 1 3.png|Untitled 1 3.png]]
![[Pasted image 20240909154821.png]]![[Pasted image 20240909154832.png]]![[Pasted image 20240909154844.png]]![[Pasted image 20240909154855.png]]

### Методы Interface Collection
![[Pasted image 20240909155018.png]]![[Pasted image 20240909155101.png]]![[Pasted image 20240909155124.png]]![[Pasted image 20240909155222.png]]
---
title: Vector and Stack
tags:
  - Collection
  - List
  - Stack
related_topics: 
created: 2024-09-09 17:08
modified: 2025-02-24T11:44:51+03:00
questions: 
notes: 
links: 
---

### Vector and Stack

[https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html)

Наследуется от AbstractList.

Это<mark class="hltr-yellow"> устаревший synchronized класс в Java</mark>, который представляет собой динамический массив, очень похожий на ArrayList. Главное о<mark class="hltr-yellow">тличие заключается в том, что Vector является потокобезопасным</mark>, в то время как ArrayList - нет. Это означает, что несколько потоков могут одновременно обращаться к методам Vector без каких-либо проблем синхронизации.

<mark class="hltr-red">Не рекомендован для использования.</mark>

Некоторые из методов, которые принадлежат только классу Vector в Java:

- `addElement(E obj)`: добавляет объект в конец вектора.
- `insertElementAt(E obj, int index)`: вставляет объект в заданную позицию вектора.
- `removeElement(Object obj)`: удаляет первое вхождение указанного объекта из вектора.
- `removeElementAt(int index)`: удаляет элемент в указанной позиции вектора.
- `removeAllElements()`: удаляет все элементы из вектора.
- `capacity()`: возвращает текущую емкость вектора.
- `trimToSize()`: уменьшает емкость вектора до его текущего размера.
- `setSize(int newSize)`: устанавливает новый размер вектора. Если новый размер больше текущего, то добавляются пустые элементы, если меньше - удаляются элементы с конца вектора.

  

### Stack наследник Vector

[https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html](https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html)

<mark class="hltr-yellow">Устаревший synchronized класс</mark>. Использует принцип LIFO. Last In First Oust.

Пример игра в дурака, с колоды берем последнюю положенную карту.

Не рекомендован для использования.

Некоторые методы Stack:

1. `push(E item)` - добавляет элемент на вершину стека.
2. `pop()` - удаляет и возвращает элемент на вершине стека. Может бросить EmptyStackException
3. `peek()` - возвращает элемент на вершине стека без его удаления. Может бросить EmptyStackException
4. `empty()` - проверяет, пуст ли стек.
5. `search(Object o)` - возвращает 1-based индекс нахождения объекта в стеке. Если объект не найден, возвращается -1.

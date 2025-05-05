---
title: LinkedList
tags:
  - Collection
  - List
related_topics: 
created: 2024-09-09 17:10
modified: 2024-12-28T12:43:04+03:00
questions: 
notes: 
links: 
---

### class LinkedList\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)\<E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)\<E>, [Deque](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)\<E>, [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)<\E>, [Queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)\<E>

  

<mark class="hltr-red">LinkedList</mark> - э<mark class="hltr-yellow">то реализация связанного списка в Java, где каждый узел списка хранит ссылку на следующий элемент в списке. Каждый узел также содержит данные, которые могут быть любого типа.</mark>

<mark class="hltr-orange">Преимущества LinkedList:</mark>

- Быстрое добавление и удаление элементов в середине списка. Это происходит за время O(1), так как не требуется перемещение других элементов, как при работе с массивами.
- Список может быть изменен в размере во время выполнения программы.
- <mark class="hltr-yellow">Быстрое добавление и удаление элементов в начале списка. Это происходит за время O(1), так как нет необходимости сдвигать другие элементы, как при работе с массивами.</mark>

<mark class="hltr-orange">Недостатки LinkedList:</mark>

- Доступ к элементам осуществляется за время O(n), где n - это индекс элемента, так как для получения доступа к элементу нужно пройти по всему списку.
- Использование памяти выше, чем при использовании массивов, так как каждый узел списка должен хранить ссылки на следующий элемент в списке.
- Индексация элементов неэффективна, так как для получения элемента по индексу нужно пройти по всему списку.

  

<mark class="hltr-orange">Наследуемые методы:</mark>

<mark class="hltr-red">LinkedList не наследует методов от ArrayList напрямую, так как они реализуют разные интерфейсы</mark> - <mark class="hltr-purple">LinkedList реализует интерфейс List, Queue и Deque</mark>, а <mark class="hltr-blue">ArrayList - только List и RandomAccess</mark>. Однако, <mark class="hltr-green2">обе коллекции реализуют базовый интерфейс Collection</mark>, поэтому многие методы у них совпадают.

  

<mark class="hltr-orange">Отличающиеся методы.</mark>

- `void` [`addFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#addFirst-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в начало списка

```Java
LinkedList<String> list = new LinkedList<>();
list.add("Alice");
list.addFirst("Bob");
System.out.println(list); // [Bob, Alice]
```

- `void` [`addLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#addLast-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в конец списка

```Java
LinkedList<String> list = new LinkedList<>();
list.add("Alice");
list.addLast("Charlie");
System.out.println(list); // [Alice, Charlie]
```

- [`descendingIterator`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#descendingIterator--)`()` определен в интерфейсе `Deque` и возвращает итератор, проходящий по элементам списка в обратном порядке (с конца списка к началу). В классе `LinkedList` данный метод реализуется следующим образом

```Java
public Iterator<E> descendingIterator() {
    return new DescendingIterator();
}

private class DescendingIterator implements Iterator<E> {
    private final ListIterator<E> itr = new ListItr(size());

    public boolean hasNext() {
        return itr.hasPrevious();
    }

    public E next() {
        return itr.previous();
    }

    public void remove() {
        itr.remove();
    }
}
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`element`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#element--)`()` возвращает первый элемент в списке, метод queue
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`getFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#getFirst--)`()` возвращает первый элемент в списке.
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`getLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#getLast--)`()` возвращает последний элементов списке
- `boolean` [`offer`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#offer-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)`и `boolean` [`offerLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#offerLast-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в конец списка.

```Java
LinkedList<String> list = new LinkedList<>();
list.offerLast("Alice");
list.offerLast("Bob");
System.out.println(list); // [Alice, Bob]
```

- `boolean` [`offerFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#offerFirst-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в начало списка

```Java
LinkedList<String> list = new LinkedList<>();
list.offerFirst("Alice");
list.offerFirst("Bob");
System.out.println(list); // [Bob, Alice]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`peek`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#peek--)`()` возвращает первый эелемент
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`peekFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#peekFirst--)`()` возвращает первый элемент
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`peekLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#peekLast--)`()` возвращает последний элемент
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`poll`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#poll--)`()` и [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`pollFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#pollFirst--)`()` удаляют и возвращают первый элемент в списке.

```Java
LinkedList<String> list = new LinkedList<>();
list.offer("Alice");
list.offer("Bob");
String first = list.pollFirst();
System.out.println(first); // Alice
System.out.println(list); // [Bob]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`pollLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#pollLast--)`()` удаляет и возвращает последний элемент в списке.

```Java
LinkedList<String> list = new LinkedList<>();
list.offer("Alice");
list.offer("Bob");
String last = list.pollLast();
System.out.println(last); // Bob
System.out.println(list); // [Alice]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`pop`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#pop--)`()` удаляет первый элемент
- `void` [`push`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#push-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) `e)` добавляет элемент в начало списка
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`removeFirst`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeFirst--)`()` удаляет первый элемент
- `boolean` [`removeFirstOccurrence`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeFirstOccurrence-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` удаляет первое вхождение

```Java
LinkedList<Integer> list = new LinkedList<>(Arrays.asList(1,2,3,4,5,6,7,8,9,10,1));
        list.removeFirstOccurrence(1);
        System.out.println(list);

[2, 3, 4, 5, 6, 7, 8, 9, 10, 1]
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html) [`removeLast`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeLast--)`()` удаляет последний элемент
- `boolean` [`removeLastOccurrence`](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html#removeLastOccurrence-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` удаляет первое вхождение, начиная с конца

```Java
LinkedList<Integer> list = new LinkedList<>(Arrays.asList(1,2,3,4,5,6,7,8,9,10,1));
        list.removeLastOccurrence(1);
        System.out.println(list);
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

  ![[images/Untitled 3 18.png|Untitled 3 18.png]]![[images/Untitled 4 18.png|Untitled 4 18.png]]![[images/Untitled 5 18.png|Untitled 5 18.png]]
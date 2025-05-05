---
title: Interface Iterator
tags:
  - Collection
related_topics:
  - "[[Почему в цикле for нельзя обновить коллекцию]]"
  - "[[Interface Spliterator]]"
  - "[[ListIterator extends Iterator]]"
created: 2024-09-09 15:56
modified: 2025-01-16T18:44:27+03:00
difficulty: 
questions: 
notes: 
links: 
---


----
[[Iterator job4j]]

----


# Interface Iterator

**Преимущества**.

- Вы <mark class="hltr-yellow">можете использовать эти итераторы для любого класса Collection.</mark>
- Поддерживает операции чтения и удаления.
- <mark class="hltr-red">Если вы используете цикл for, вы не можете обновить (добавить / удалить) коллекцию</mark>, тогда как <mark class="hltr-green2">с помощью итератора вы можете легко обновить коллекцию.</mark>  [[Почему в цикле for нельзя обновить коллекцию]]
- Универсальный для API коллекции.
- Удаление с помощью итератора в LinkedList происходит за const

**Недостатки:**

- Вы можете выполнять только итерацию в прямом направлении, то есть <mark class="hltr-yellow">однонаправленный итератор.</mark>
- Замена и добавление нового элемента не поддерживается.
- <mark class="hltr-red">ListIterator</mark> – <mark class="hltr-yellow">самый мощный, но он применим только для классов, реализованных в List.</mark>
- Когда вы используете операции CRUD, он не поддерживает операции создания и обновления.
- Когда вы сравниваете его со Spliterator, он не позволяет выполнять итерацию элементов параллельно. Это означает, что он поддерживает только последовательную итерацию.
- <mark class="hltr-yellow">Он не поддерживает лучшую производительность для итерации большого объема данных.</mark>

### Методы

- `default void` [`forEachRemaining`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#forEachRemaining-java.util.function.Consumer-)`(`[`Consumer`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html)`<? super` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)`> action)` позволяет применить заданный метод к каждому элементу коллекции, оставшемуся после текущей позиции итератора.

```Java
List<String> list = new ArrayList<>();
list.add("one");
list.add("two");
list.add("three");

Iterator<String> iterator = list.iterator();
iterator.next(); // пропускаем первый элемент

iterator.forEachRemaining(System.out::println); // выводим оставшиеся элементы
```

- `boolean` [`hasNext`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#hasNext--)`()` возвращает `true`, если в коллекции есть следующий элемент, и `false`, если элементов больше нет.
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html) [`next`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#next--)`()`возвращает следующий элемент коллекции. Если в коллекции больше нет элементов, будет выброшено исключение `NoSuchElementException`.
- `default void` [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#remove--)`()` удаляет текущий элемент из коллекции. Если метод `next()` не был вызван после последнего вызова `remove()`, будет выброшено исключение `IllegalStateException`.

```Java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);

// получаем итератор для списка
Iterator<Integer> iterator = list.iterator();

// проверяем наличие следующего элемента и выводим его на экран
while (iterator.hasNext()) {
    Integer element = iterator.next();
    System.out.println(element);
    
    // удаляем текущий элемент из списка
    iterator.remove();
}
```



![[images/Untitled 6 17.png|Untitled 6 17.png]]![[images/Untitled 7 16.png|Untitled 7 16.png]]![[images/Untitled 8 15.png|Untitled 8 15.png]]
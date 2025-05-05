---
title: Interface Iterable
tags:
  - Collection
related_topics:
  - "[[Interface Iterator]]"
  - "[[Interface Collection]]"
created: 2024-09-09 15:53
modified: 2024-09-10T13:44:15+03:00
difficulty: 
questions: 
notes: 
links: 
---
# Interface Iterable
<mark class="hltr-yellow">Является корневым</mark> для всех коллекций в Java.<mark class="hltr-red"> Он объявляет один метод, который должны реализовывать все его подклассы.</mark>

<mark class="hltr-green2">Позволяет итерироваться по коллекции с помощью for-Each.</mark>

- [`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)`<`[`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)`>` [`iterator`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#iterator--)`()` возвращает итератор, который можно использовать для последовательного перебора элементов коллекции

```Java
List<String> list = Arrays.asList("one", "two", "three");

Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String str = it.next();
    System.out.println(str);
}
```

- `default void` [`forEach`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#forEach-java.util.function.Consumer-)`(`[`Consumer`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html)`<? super` [`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)`> action)` метод выполняет указанное действие для каждого элемента коллекции

```Java
List<String> list = Arrays.asList("one", "two", "three");

list.forEach(str -> System.out.println(str));
```

- `default` [`Spliterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html)`<`[`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)`>` [`spliterator`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#spliterator--)`()` метод возвращает `Spliterator`, который можно использовать для параллельного перебора элементов коллекции. `Spliterator` является абстракцией для параллельного перебора элементов коллекции

```Java
List<String> list = Arrays.asList("one", "two", "three");

Spliterator<String> spliterator = list.spliterator();
spliterator.trySplit().forEachRemaining(str -> System.out.println(str));
```

![[Pasted image 20240909155559.png]]
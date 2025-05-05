---
title: Interface Spliterator
tags:
  - Collection
related_topics: 
created: 2024-09-09 16:36
modified: 2024-09-09T16:50:00+03:00
questions: 
notes: 
links: 
---
## Interface Spliterator\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html](https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html)

<mark class="hltr-red">Spliterator</mark> (split-iterator) - это интерфейс,<mark class="hltr-yellow"> который был добавлен в Java 8 в пакет java.util</mark>. Он представляет собой н<mark class="hltr-green2">овый тип итератора, который может рекурсивно разбивать коллекцию на несколько фрагментов для обработки в параллельном режиме.</mark>

Основное отличие <mark class="hltr-yellow">между Spliterator и обычным итератором заключается в возможности разделить коллекцию на несколько частей для параллельной обработки</mark>, что может значительно ускорить процесс выполнения операций.

Сам Spliterator имеет несколько методов, которые нужны для выполнения задачи разбиения коллекции на части. Один из таких методов - это <mark class="hltr-red">trySplit</mark> (), который <mark class="hltr-yellow">разделяет итерируемый объект на две части</mark>. Результатом вызова этого метода является новый Spliterator, который может обрабатывать одну из двух частей, в то время как первый Spliterator обрабатывает другую часть.

Другие методы Spliterator включают методы для проверки оставшихся элементов в итерируемой коллекции, для выполнения операции для каждого элемента, для оценки количества элементов и т.д.

Вот небольшой пример использования Spliterator на Java:

```Java
List<Integer> myList = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
        Spliterator<Integer> mySplitIterator = myList.spliterator();
        mySplitIterator.trySplit().forEachRemaining(System.out::print); //вывод:12345
```

Кроме того, <mark class="hltr-yellow">Spliterator используется во многих других классах Java Collections API</mark>, таких как <mark class="hltr-blue">HashSet, TreeSet, ArrayList и т.д</mark>. В некоторых случаях, когда требуется многопоточная обработка данных, использование Spliterator может быть значительно быстрее, чем использование обычного итератора.

### Методы

- `tryAdvance(Consumer<? super T> action)`: <mark class="hltr-yellow">Если остался ещё элемент, выполняет заданное действие на нём и возвращает</mark> `true`; иначе возвращает `false`.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
while (spliterator.tryAdvance(System.out::println)) {
    // Выводит в консоль элементы: "a", "b", "c"
}
```

- `trySplit()`: <mark class="hltr-yellow">Если этот сплитератор может быть разделён на части, возвращает новый сплитератор, который охватывает элементы, которые не будут охвачены этим сплитератором после вызова этого метода.</mark>

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
Spliterator<String> newSpliterator = spliterator.trySplit();
if (newSpliterator != null) {
    newSpliterator.forEachRemaining(System.out::println); // Выводит в консоль элементы: "b", "c"
}
```

- `forEachRemaining(Consumer<? super T> action)`:<mark class="hltr-green2"> Выполняет заданное действие для каждого оставшегося элемента, последовательно в текущем потоке, пока все элементы не будут обработаны или действие не вызовет исключение.</mark>

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
spliterator.forEachRemaining(System.out::println); // Выводит в консоль элементы: "a", "b", "c"
```

- `estimateSize()`: <mark class="hltr-yellow">Возвращает оценку количества элементов, которые будут найдены при обходе </mark>`forEachRemaining(Consumer<? super T> action)`, или возвращает `Long.MAX_VALUE`, если количество элементов бесконечно, неизвестно или вычисляется слишком долго.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
System.out.println(spliterator.estimateSize()); // Выводит в консоль "3"
```

- `getExactSizeIfKnown()`: Удобный метод, <mark class="hltr-yellow">который возвращает</mark> `estimateSize()`, <mark class="hltr-yellow">если</mark> этот <mark class="hltr-yellow">сплитератор имеет размер</mark>, иначе возвращает `1`.

```Java
Spliterator<String> spliterator = Arrays.asList("a", "b", "c").spliterator();
System.out.println(spliterator.getExactSizeIfKnown()); // Выводит в консоль "3"
```

- `getComparator()`: Если источник этого сплитератора отсортирован с помощью компаратора, возвращает этот компаратор; иначе возвращает `null`.

```Java
Spliterator<String> spliterator = Arrays.asList("c", "a", "b").spliterator();
Comparator<String> comparator = spliterator.getComparator(); // Возвращает null
```

- `hasCharacteristics(int characteristics)`: возвращает `true`, если у данного сплитератора все характеристики, указанные в параметре `characteristics`.  
    Характеристики описывают особенности сплитератора и его элементов,  
    такие как имеющиеся у них свойства или ограничения. Данный метод может  
    использоваться, например, для проверки того, что сплитератор является  
    упорядоченным (  
    `ORDERED`), неизменяемым (`IMMUTABLE`) или сортированным (`SORTED`).

```Java
ArrayList<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
Spliterator<Integer> spliterator = list.spliterator();

// Проверяем, содержит ли Spliterator характеристики ORDERED и SIZED
if (spliterator.hasCharacteristics(Spliterator.ORDERED | Spliterator.SIZED)) {
    System.out.println("Spliterator содержит характеристики ORDERED и SIZED");
} else {
    System.out.println("Spliterator не содержит характеристики ORDERED и SIZED");
}
```


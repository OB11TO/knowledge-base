---
title: ListIterator extends Iterator
tags:
  - Collection
related_topics: 
created: 2024-09-09 16:11
modified: 2025-02-24T11:39:02+03:00
difficulty: 
questions: 
notes: 
links: 
---

### ListIterator extends Iterator

[https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html)

<mark class="hltr-yellow">Интерфейс, расширяющий интерфейс </mark>`Iterator`, <mark class="hltr-green2">предназначенный для перебора элементов списков</mark>. `ListIterator` <mark class="hltr-red">обладает дополнительными методами, позволяющими осуществлять двунаправленный проход по списку и изменять его элементы.</mark>

### Методы

<mark class="hltr-orange">Наследуемые от Iterator:</mark>

- `boolean` [`hasNext`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#hasNext--)`()` возвращает `true`, если в коллекции есть следующий элемент, и `false`, если элементов больше нет.
- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html) [`next`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#next--)`()`возвращает следующий элемент коллекции. Если в коллекции больше нет элементов, будет выброшено исключение `NoSuchElementException`.
- `default void` [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html#remove--)`()` удаляет текущий элемент из коллекции. Если метод `next()` не был вызван после последнего вызова `remove()`, будет выброшено исключение `IllegalStateException`.



<mark class="hltr-purple">Собственные методы:</mark>

`void` [`add`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#add-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html) `e)` добавляет указанный элемент в список между элементами, который будет возвращен следующим вызовом `next()` или `previous()`.

`boolean` [`hasPrevious`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#hasPrevious--)`()` возвращает `true`, если список имеет предыдущий элемент, и `false`, если элементов больше нет.

`int` [`nextIndex`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#nextIndex--)`()` возвращает индекс следующего элемента.

[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html) [`previous`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#previous--)`()` возвращает предыдущий элемент списка. Если в списке больше нет элементов, будет выброшено исключение `NoSuchElementException`.

`int` [`previousIndex`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#previousIndex--)`()` возвращает индекс предыдущего элемента, если он имеется.

`void` [`set`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html#set-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html) `e)` заменяет последний элемент, который был возвращен методом `next()` или `previous()`, на указанный элемент.

```Java
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;
import java.util.NoSuchElementException;

public class ListIteratorExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple");
        list.add("banana");
        list.add("cherry");

        ListIterator<String> iterator = list.listIterator();

        // перебор списка вперед и вывод на экран
        System.out.println("Перебор списка вперед:");
        while (iterator.hasNext()) {
            int nextIndex = iterator.nextIndex();
            String nextElement = iterator.next();
            int previousIndex = iterator.previousIndex();
            System.out.println("  nextIndex=" + nextIndex + ", previousIndex=" + previousIndex + ", nextElement=" + nextElement);
        }

        // перебор списка назад и вывод на экран
        System.out.println("Перебор списка назад:");
        while (iterator.hasPrevious()) {
            int nextIndex = iterator.nextIndex();
            int previousIndex = iterator.previousIndex();
            String previousElement = iterator.previous();
            System.out.println("  nextIndex=" + nextIndex + ", previousIndex=" + previousIndex + ", previousElement=" + previousElement);
        }

        // замена второго элемента на "orange"
        iterator = list.listIterator();
        while (iterator.hasNext()) {
            String nextElement = iterator.next();
            if (nextElement.equals("banana")) {
                int previousIndex = iterator.previousIndex();
                System.out.println("Замена элемента на позиции " + previousIndex + " на \"orange\"");
                iterator.set("orange");
                break;
            }
        }

        // добавление элемента "mango" после "orange"
        iterator.add("mango");
        int previousIndex = iterator.previousIndex();
        System.out.println("Добавление элемента \"mango\" на позицию " + previousIndex);

        // вывод на экран измененного списка
        System.out.println("Измененный список:");
        for (String element : list) {
            System.out.println("  " + element);
        }
    }
}
```

![[Pasted image 20240909162005.png]]
---
title: HashSet
tags:
  - Collection
  - Set
related_topics: 
created: 2024-09-09 17:27
modified: 2024-09-10T12:42:06+03:00
questions: 
notes: 
links: 
---
# class HashSet\<E>

<mark class="hltr-red">HashSet</mark> <mark class="hltr-yellow">не предоставляет прямого способа получить значение по индексу, поскольку значения в HashSet хранятся в неупорядоченном вид</mark>е. Если вам требуется получить значение по определенному индексу, вам может потребоваться использовать другую реализацию, такую как ArrayList или LinkedList.

В HashSet значения хранятся в неупорядоченном виде и доступ к ним осуществляется с использованием метода `contains()` или `iterator()`.

<mark class="hltr-orange">Плюсы</mark>:

1. Быстрый доступ: Добавление, удаление и поиск элементов в HashSet выполняются очень быстро. Это происходит благодаря использованию хэш-таблицы, которая обеспечивает эффективный доступ к элементам по хэш-коду.
2. Уникальность элементов: HashSet гарантирует, что каждый элемент в наборе является уникальным. Если вы пытаетесь добавить дублирующийся элемент, он будет проигнорирован.
3. Нет порядка элементов: В HashSet элементы не упорядочены и могут располагаться внутри структуры данных в произвольном порядке. Если вам не требуется определенный порядок элементов, HashSet может быть хорошим выбором.
4. Высокая производительность: HashSet обладает хорошей производительностью при больших объемах данных. Операции добавления, удаления и поиска имеют почти постоянное время выполнения, что делает HashSet эффективным для работы с большими наборами данных.

<mark class="hltr-orange">Минусы</mark>:

1. Отсутствие гарантированного порядка: В HashSet нет гарантии относительного порядка элементов. Порядок может изменяться при изменении набора данных или перехэшировании хэш-таблицы. Если вам требуется сохранение порядка элементов, вам следует рассмотреть другие реализации Set, такие как LinkedHashSet.
2. Неупорядоченный итератор: Итератор HashSet не гарантирует определенный порядок обхода элементов. Порядок итерации могут влиять на внутренние механизмы хэш-таблицы, поэтому не стоит полагаться на порядок элементов при обходе набора.
3. Неэффективное использование памяти: HashSet может использовать больше памяти, чем другие реализации Set, особенно если в нем содержится большое количество элементов или элементы занимают много места в памяти. Это связано с необходимостью выделения памяти для хранения хэш-таблицы.
4. Затраты на хеширование: При использовании HashSet происходит вычисление хэш-кода для каждого добавленного элемента. В некоторых случаях это может быть затратной операцией.

### Методы

- `boolean add(E e)` Добавляет указанный элемент в множество, если его еще нет.
- `void clear()`Удаляет все элементы из множества.
- `Object clone()`Возвращает поверхностную копию данного экземпляра HashSet: элементы сами по себе не клонируются.
- `boolean contains(Object o)`Возвращает true, если множество содержит указанный элемент.
- `boolean isEmpty()`Возвращает true, если множество не содержит элементов.
- `Iterator<E> iterator()`Возвращает итератор по элементам множества.
- `boolean remove(Object o)`Удаляет указанный элемент из множества, если он присутствует.
- `int size()`Возвращает количество элементов в множестве (его мощность).
- `Spliterator<E> spliterator()`Создает late-binding и fail-fast Spliterator по элементам множества.

```Java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class HashSetExample {
    public static void main(String[] args) {
        // Создание экземпляра HashSet
        Set<String> set = new HashSet<>();

        // Добавление элементов в множество
        set.add("яблоко");
        set.add("банан");
        set.add("апельсин");

        // Вывод размера множества
        System.out.println("Размер множества: " + set.size());

        // Проверка наличия элемента в множестве
        boolean containsBanana = set.contains("банан");
        System.out.println("Множество содержит банан? " + containsBanana);

        // Итерация по элементам множества с использованием итератора
        Iterator<String> iterator = set.iterator();
        System.out.println("Элементы множества:");
        while (iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
        }

        // Удаление элемента из множества
        boolean removed = set.remove("яблоко");
        System.out.println("Элемент 'яблоко' удален? " + removed);

        // Очистка множества
        set.clear();
        System.out.println("Множество после очистки: " + set);
    }
}
```

### Methods inherited from class java.util.[AbstractSet](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html)

- [`equals`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html#equals-java.lang.Object-)`,` [`hashCode`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html#hashCode--)`,` [`removeAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html#removeAll-java.util.Collection-)

### Methods inherited from class java.util.[AbstractCollection](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html)

- [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#addAll-java.util.Collection-)`,` [`containsAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#containsAll-java.util.Collection-)`,` [`retainAll`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#retainAll-java.util.Collection-)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#toArray--)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#toArray-T:A-)`,` [`toString`](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractCollection.html#toString--)

### **Methods inherited from class java.lang.**[**Object**](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)

- [`finalize`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#finalize--)`,` [`getClass`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#getClass--)`,` [`notify`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notify--)`,` [`notifyAll`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#notifyAll--)`,` [`wait`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait--)`,` [`wait`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait-long-)`,` [`wait`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#wait-long-int-)

### **Methods inherited from interface java.util.**[**Set**](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)

- [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#addAll-java.util.Collection-)`,` [`containsAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#containsAll-java.util.Collection-)`,` [`equals`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#equals-java.lang.Object-)`,` [`hashCode`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#hashCode--)`,` [`removeAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#removeAll-java.util.Collection-)`,` [`retainAll`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#retainAll-java.util.Collection-)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#toArray--)`,` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html#toArray-T:A-)

### Methods inherited from interface java.util.[Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)

- [`parallelStream`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#parallelStream--)`,` [`removeIf`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#removeIf-java.util.function.Predicate-)`,` [`stream`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#stream--)

### **Methods inherited from interface java.lang.**[**Iterable**](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)

- [`forEach`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html#forEach-java.util.function.Consumer-)

---
title: java.util.Collections
tags:
  - Collection
related_topics: 
created: 2024-09-09 18:19
modified: 2024-09-09T18:19:58+03:00
questions: 
notes: 
links: 
---
# Class java.util.Collections

[https://docs.oracle.com/javase/8/docs/api/?java/util/Collections.html](https://docs.oracle.com/javase/8/docs/api/?java/util/Collections.html)

Это утилитный класс, предоставляющий набор методов для работы с коллекциями. В нем содержатся методы для сортировки, перемешивания, копирования, поиска элементов и другие методы, упрощающие работу с коллекциями.

### Fields

- `static` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) [`EMPTY_LIST`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#EMPTY_LIST)
- `static` [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html) [`EMPTY_MAP`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#EMPTY_MAP)
- `static` [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html) [`EMPTY_SET`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#EMPTY_SET)

### Методы

- `static <T> boolean addAll(Collection<? super T> c, T... elements)`: Добавляет все указанные элементы в указанную коллекцию.

```Java
List<String> list = new ArrayList<>();
Collections.addAll(list, "foo", "bar", "baz");
System.out.println(list); // Output: [foo, bar, baz]
```

- `static <T>` [`Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)`<T> asLifoQueue(Deque<T> deque)`: Возвращает представление Deque в виде очереди Last-in-first-out (Lifo).

```Java
Deque<Integer> deque = new ArrayDeque<>();
deque.push(1);
deque.push(2);
deque.push(3);
Queue<Integer> lifoQueue = Collections.asLifoQueue(deque);
System.out.println(lifoQueue.poll()); // Вывод: 3
System.out.println(lifoQueue.poll()); // Вывод: 2
System.out.println(lifoQueue.poll()); // Вывод: 1
```

- `static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)`: Ищет указанный объект в указанном списке с использованием алгоритма бинарного поиска.
- `static <T> int binarySearch(List<? extends T> list, T key, Comparator<? super T> c)`: Ищет указанный объект в указанном списке с использованием алгоритма бинарного поиска и сравнивает элементы с помощью указанного компаратора.
- `static <E>` [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<E> checkedCollection(Collection<E> c, Class<E> type)`: Возвращает динамически типизированное представление указанной коллекции.
- `static <E>` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<E> checkedList(List<E> list, Class<E> type)`: Возвращает динамически типизированное представление указанного списка.

```Java
List<Student> students = new ArrayList<>();
List<Student> checkedStudents = Collections.checkedList(students, Student.class);
Теперь при попытке добавления объекта,
который не является типом Student в список checkedStudents,
будет сгенерировано исключение ClassCastException.
```

- `static <K,V>` [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<K,V> checkedMap(Map<K,V> m, Class<K> keyType, Class<V> valueType)`: Возвращает динамически типизированное представление указанной карты.
- `static <K,V>` [`NavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html)`<K,V> checkedNavigableMap(NavigableMap<K,V> m, Class<K> keyType, Class<V> valueType)`: Возвращает динамически типизированное представление указанной упорядоченной карты.
- `static <E>` [`NavigableSet`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html)`<E> checkedNavigableSet(NavigableSet<E> s, Class<E> type)`: Возвращает динамически типизированное представление указанного упорядоченного набора.
- `static <E>` [`Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)`<E> checkedQueue(Queue<E> queue, Class<E> type)`: Возвращает динамически типизированное представление указанной очереди.
- `static <E>` [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)`<E> checkedSet(Set<E> s, Class<E> type)`: Возвращает динамически типизированное представление указанного набора.
- `static <K,V>` [`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)`<K,V> checkedSortedMap(SortedMap<K,V> m, Class<K> keyType, Class<V> valueType)`: Возвращает динамически типизированное представление указанной упорядоченной карты.
- `static <E>` [`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)`<E> checkedSortedSet(SortedSet<E> s, Class<E> type)`: Возвращает динамически типизированное представление указанного упорядоченного набора.
- `static <T> void copy(List<? super T> dest, List<? extends T> src)`: Копирует все элементы из одного списка в другой.

```Java
List<Integer> srcList = Arrays.asList(1, 2, 3);
List<Number> destList = new ArrayList<>(Arrays.asList(0, 0, 0));

Collections.copy(destList, srcList);

System.out.println(destList); // Output: [1, 2, 3]
```

- `static boolean disjoint(Collection<?> c1, Collection<?> c2)`: Возвращает true, если две указанные коллекции не имеют общих элементов.

```Java
List<Integer> list1 = Arrays.asList(1, 2, 3);
List<Integer> list2 = Arrays.asList(4, 5, 6);

if (Collections.disjoint(list1, list2)) {
    System.out.println("Коллекции не имеют общих элементов"); // сработает это
} else {
    System.out.println("Коллекции имеют общие элементы"); 
}
```

- `static <T>` [`Enumeration`](https://docs.oracle.com/javase/8/docs/api/java/util/Enumeration.html)`<T> emptyEnumeration()`: Возвращает перечисление, которое не имеет элементов.
- `static <T>` [`Iterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html)`<T> emptyIterator()`: Возвращает итератор, который не имеет элементов.
- `static <T>` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<T> emptyList()`: Возвращает пустой список (неизменяемый).
- `static <T>` [`ListIterator`](https://docs.oracle.com/javase/8/docs/api/java/util/ListIterator.html)`<T> emptyListIterator()`: Возвращает итератор списка без элементов.
- `static <K,V>` [`Map`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)`<K,V> emptyMap()`: Возвращает пустую карту (неизменяемую).
- `static <K,V>` [`NavigableMap`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html)`<K,V> emptyNavigableMap()`: Возвращает пустую навигационную карту (неизменяемую).
- `static <E>` [`NavigableSet`](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableSet.html)`<E> emptyNavigableSet()`: Возвращает пустой навигационный набор (неизменяемый).
- `static <T>` [`Set`](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)`<T> emptySet()`: Возвращает пустой набор (неизменяемый).
- `static <K,V>` [`SortedMap`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)`<K,V> emptySortedMap()`: Возвращает пустую отсортированную карту (неизменяемую).
- `static <E>` [`SortedSet`](https://docs.oracle.com/javase/8/docs/api/java/util/SortedSet.html)`<E> emptySortedSet()`: Возвращает пустой отсортированный набор (неизменяемый).
- `static <T>` [`Enumeration`](https://docs.oracle.com/javase/8/docs/api/java/util/Enumeration.html)`<T> enumeration(Collection<T> c)`: Возвращает перечисление по указанной коллекции.
- `static <T> void fill(List<? super T> list, T obj)`: Заменяет все элементы указанного списка указанным элементом.

```Java
List<String> list = new ArrayList<>(Arrays.asList("foo", "bar", "baz"));
Collections.fill(list, "hello");
System.out.println(list); // выведет [hello, hello, hello]
```

- `static int frequency(Collection<?> c, Object o)`: Возвращает количество элементов в указанной коллекции, равных указанному объекту.
- `static int indexOfSubList(List<?> source, List<?> target)`: Возвращает начальную позицию первого вхождения указанного целевого списка в указанном исходном списке, или -1, если такое вхождение отсутствует.

```Java
List<Integer> source = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
List<Integer> target = Arrays.asList(3, 4, 5);

int index = Collections.indexOfSubList(source, target);

System.out.println(index); // Output: 2
```

- `static int lastIndexOfSubList(List<?> source, List<?> target)`: Возвращает начальную позицию последнего вхождения указанного целевого списка в указанном исходном списке, или -1, если такое вхождение отсутствует.
- `static <T>` [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`<T> list(Enumeration<T> e)`: Возвращает список ArrayList, содержащий элементы, возвращаемые указанным перечислением в порядке, в котором они возвращаются перечислением.
- `static <T extends` [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `&` [`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)`<? super T>> T max(Collection<? extends T> coll)`: Возвращает максимальный элемент данной коллекции в соответствии с естественным порядком элементов.
- `static <T> T max(Collection<? extends T> coll, Comparator<? super T> comp)`: Возвращает максимальный элемент данной коллекции в соответствии с порядком, вызываемым указанным компаратором.
- `static <T extends` [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `&` [`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)`<? super T>> T min(Collection<? extends T> coll)`: Возвращает минимальный элемент данной коллекции в соответствии с естественным порядком элементов.
- `static <T> T min(Collection<? extends T> coll, Comparator<? super T> comp)`: Возвращает минимальный элемент данной коллекции в соответствии с порядком, вызываемым указанным компаратором.
- `static <T>` [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<T> nCopies(int n, T o)`: Возвращает неизменяемый список, состоящий из n копий указанного объекта.

```Java
List<String> list = Collections.nCopies(3, "Hello");
System.out.println(list); // Output: [Hello, Hello, Hello]
```

- `static <E> Set<E> newSetFromMap(Map<E,Boolean> map)` - возвращает новый `Set` с поддержкой предоставленного `Map`. В `Set` будут содержаться все ключи, которые имеют `Boolean` значение `true` в указанной `Map`. Это означает, что при добавлении ключа в этот `Set`, его значение в `Map` будет установлено в `true`, а при удалении ключа из `Set`, его значение в `Map` будет установлено в `false`.

```Java
Map<String, Boolean> map = new HashMap<>();
Set<String> set = Collections.newSetFromMap(map);

set.add("foo");
set.add("bar");

System.out.println(map); // выводит {bar=true, foo=true}

set.remove("foo");

System.out.println(map); // выводит {bar=true, foo=false}
```

- `static <T> boolean replaceAll(List<T> list, T oldVal, T newVal)` - заменяет все вхождения одного заданного значения в списке на другое.

```Java
List<String> list = new ArrayList<>(Arrays.asList("apple", "banana", "cherry"));
System.out.println("Before replaceAll: " + list); // [apple, banana, cherry]
Collections.replaceAll(list, "banana", "orange");
System.out.println("After replaceAll: " + list); // [apple, orange, cherry]
```

- `static void reverse(List<?> list)` - переворачивает порядок элементов в указанном списке.

```Java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);
        
        System.out.println("Original list: " + list);
        
        Collections.reverse(list);
        
        System.out.println("Reversed list: " + list);
    }
}
Original list: [1, 2, 3, 4, 5]
Reversed list: [5, 4, 3, 2, 1]
```

- `static <T> Comparator<T> reverseOrder()` - возвращает компаратор, который накладывает обратный порядок естественного упорядочивания на коллекцию объектов, реализующих интерфейс Comparable.
- `static <T> Comparator<T> reverseOrder(Comparator<T> cmp)` - возвращает компаратор, который накладывает обратный порядок указанного компаратора.

```Java
List<Integer> numbers = Arrays.asList(5, 3, 8, 1, 9, 2);
Comparator<Integer> reverseComparator = Collections.reverseOrder();
Collections.sort(numbers, reverseComparator);
System.out.println(numbers); // [9, 8, 5, 3, 2, 1]
```

- `static void rotate(List<?> list, int distance)` - перемещает элементы в указанном списке на указанное расстояние.

```Java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
System.out.println("Before rotation: " + numbers);

Collections.rotate(numbers, 2);
System.out.println("After rotation: " + numbers);

Collections.rotate(numbers, -2);
System.out.println("After rotation: " + numbers);

Before rotation: [1, 2, 3, 4, 5]
After rotation: [4, 5, 1, 2, 3]
After rotation: [1, 2, 3, 4, 5]
```

- `static void shuffle(List<?> list)` - случайным образом переставляет указанный список, используя по умолчанию источник случайности.
- `static void shuffle(List<?> list, Random rnd)` - случайным образом переставляет указанный список, используя указанный источник случайности.

```Java
import java.util.*;

public class ShuffleExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            numbers.add(i);
        }

        System.out.println("Original List: " + numbers);

        Collections.shuffle(numbers);

        System.out.println("Shuffled List: " + numbers);
    }
}

Original List: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Shuffled List: [3, 6, 2, 4, 9, 8, 1, 10, 5, 7]
```

- `static <T> Set<T> singleton(T o)` - возвращает неизменяемый набор, содержащий только указанный объект.
- `static <T> List<T> singletonList(T o)` - возвращает неизменяемый список, содержащий только указанный объект.
- `static <K,V> Map<K,V> singletonMap(K key, V value)` - возвращает неизменяемое отображение, отображающее только указанный ключ на указанное значение. Попытка добавления новых элементов в созданный методом `singletonMap` `Map` приведет к `UnsupportedOperationException`, так как возвращаемый `Map` является неизменяемым.

```Java
Map<String, Integer> map = Collections.singletonMap("a", 1);
System.out.println(map); // выводит: {a=1}
```

- `static <T extends Comparable<? super T>> void sort(List<T> list)` - сортирует указанный список в порядке возрастания, в соответствии с естественным упорядочиванием его элементов.
- `static <T> void sort(List<T> list, Comparator<? super T> c)` - сортирует указанный список в соответствии с порядком, вызываемым указанным компаратором.

```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();
        people.add(new Person("Alice", 30));
        people.add(new Person("Bob", 25));
        people.add(new Person("Charlie", 40));
        
        Comparator<Person> byAgeDescending = (p1, p2) -> p2.getAge() - p1.getAge();
        Collections.sort(people, byAgeDescending);
        
        System.out.println(people); // [Charlie (40), Alice (30), Bob (25)]
    }
}

class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}
```

- `static void swap(List<?> list, int i, int j)` - меняет местами элементы на указанных позициях в указанном списке.

```Java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("c");

        System.out.println(list); // [a, b, c]
        Collections.swap(list, 0, 2);
        System.out.println(list); // [c, b, a]
    }
}
```

- `static <T> Collection<T> synchronizedCollection(Collection<T> c)` - возвращает синхронизированную (потокобезопасную) коллекцию, поддерживаемую указанной коллекцией.
- `static <T> List<T> synchronizedList(List<T> list)` - возвращает синхронизированный (потокобезопасный) список, поддерживаемый указанным списком.
- `static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)` - возвращает синхронизированную (thread-safe) map, которая поддерживается указанной map.
- `static <K,V> NavigableMap<K,V> synchronizedNavigableMap(NavigableMap<K,V> m)` - возвращает синхронизированную (thread-safe) NavigableMap, которая поддерживается указанной NavigableMap.
- `static <T> NavigableSet<T> synchronizedNavigableSet(NavigableSet<T> s)` - возвращает синхронизированную (thread-safe) NavigableSet, которая поддерживается указанным NavigableSet.
- `static <T> Set<T> synchronizedSet(Set<T> s)` - возвращает синхронизированный (thread-safe) set, который поддерживается указанным set.
- `static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m)` - возвращает синхронизированный (thread-safe) sorted map, который поддерживается указанным sorted map.
- `static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s)` - возвращает синхронизированный (thread-safe) sorted set, который поддерживается указанным sorted set.
- `static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c)` - возвращает неизменяемое представление указанной коллекции.
- `static <T> List<T> unmodifiableList(List<? extends T> list)` - возвращает неизменяемое представление указанного списка.

```Java
List<Person> persons = Collections.unmodifiableList(new ArrayList<Person>());
persons.add(new Person("Vadim",1000));
//Exception in thread "main" java.lang.UnsupportedOperationException
```

- `static <K,V> Map<K,V> unmodifiableMap(Map<? extends K,? extends V> m)` - возвращает неизменяемое представление указанной map.
- `static <K,V> NavigableMap<K,V> unmodifiableNavigableMap(NavigableMap<K,? extends V> m)` - возвращает неизменяемое представление указанной NavigableMap.
- `static <T> NavigableSet<T> unmodifiableNavigableSet(NavigableSet<T> s)` - возвращает неизменяемое представление указанного NavigableSet.
- `static <T> Set<T> unmodifiableSet(Set<? extends T> s)` - возвращает неизменяемое представление указанного set.
- `static <K,V> SortedMap<K,V> unmodifiableSortedMap(SortedMap<K,? extends V> m)` - возвращает неизменяемое представление указанной sorted map.
- `static <T> SortedSet<T> unmodifiableSortedSet(SortedSet<T> s)` - возвращает неизменяемое представление указанного sorted set.
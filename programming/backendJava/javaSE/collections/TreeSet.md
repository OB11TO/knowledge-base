---
title: TreeSet
tags:
  - Collection
  - Set
related_topics: 
created: 2024-09-09 17:35
modified: 2024-09-10T13:57:59+03:00
questions: 
notes: 
links: 
---
### class TreeSet<\E>

Это реализация интерфейса Set в Java,<mark class="hltr-yellow"> основанная на красно-черном дереве</mark>. Вот список плюсов и минусов использования TreeSet:

<mark class="hltr-orange">Плюсы</mark>:

1. Сортировка элементов: TreeSet <mark class="hltr-yellow">автоматически сортирует элементы по их естественному порядку или заданному компаратору</mark>. Это позволяет получить элементы в упорядоченном виде при итерации или при выполнении операций поиска.
2. Поддержка навигации: TreeSet предоставляет методы для выполнения операций, связанных с навигацией по элементам, таких как получение наименьшего или наибольшего элемента, поиск элемента, нахождение ближайшего элемента и другие.
3. Использование пользовательского компаратора: TreeSet позволяет указать свой компаратор для сортировки элементов. Это позволяет гибко определить порядок сортировки, даже для типов данных, не реализующих интерфейс Comparable.
4. Потокобезопасность: Если TreeSet используется в многопоточной среде, можно использовать блокировку или другие механизмы синхронизации для обеспечения потокобезопасности при доступе к TreeSet.

<mark class="hltr-orange">Минусы</mark>:

1. Более медленная производительность: TreeSet <mark class="hltr-yellow">обеспечивает относительно медленную производительность по сравнению с хэш-таблицами, используемыми в HashSe</mark>t. Вставка, удаление и поиск элементов требуют времени<mark class="hltr-red"> O(log n)</mark>, где n - количество элементов в TreeSet.
2. Ограничения на типы элементов: Для корректной работы TreeSet требуется, чтобы элементы были сравнимы. Это может быть проблемой для пользовательских типов данных, которые не реализуют интерфейс Comparable или не предоставляют компаратор.
3. Затраты на память: TreeSet требует д<mark class="hltr-yellow">ополнительной памяти для хранения структуры дерева. </mark>Каждый элемент требует дополнительных ссылок и узлов для организации дерева, что может привести к увеличению потребления памяти по сравнению с другими реализациями Set.
4. **Невозможность хранить `null`**: `TreeSet` не поддерживает `null`, так как все элементы должны быть сравнимы между собой, и сравнение с `null` приведет к `NullPointerException`.


# TreeSet в Java

`TreeSet` — это реализация интерфейса `Set`, которая использует красно-черное дерево для хранения элементов. Элементы в `TreeSet` автоматически сортируются, и эта коллекция предоставляет возможность выполнять операции с упорядоченными множествами.

## Основные характеристики `TreeSet`

1. **Упорядоченность**: Элементы в `TreeSet` хранятся в отсортированном порядке по возрастанию. Можно также задать кастомный порядок, используя компаратор.
   
2. **Отсутствие дубликатов**: Как и все множества в Java, `TreeSet` не допускает наличия одинаковых элементов.

3. **Высокая производительность на чтение и запись**: Основные операции (`add`, `remove`, `contains`) выполняются за логарифмическое время `O(log n)`, благодаря внутренней реализации на основе сбалансированного дерева (красно-черного дерева).

4. **Поддержка диапазонов**: `TreeSet` поддерживает операции для работы с подмножествами и диапазонами, например, можно получить элементы, попадающие в определенный интервал.

5. **Невозможно хранить `null`**: В отличие от `HashSet`, в `TreeSet` нельзя добавлять `null`, так как сравнение с `null` приведет к исключению `NullPointerException`.

## Пример использования

### 1. Создание и базовые операции

```java
import java.util.TreeSet;

public class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<Integer> numbers = new TreeSet<>();

        // Добавление элементов
        numbers.add(5);
        numbers.add(2);
        numbers.add(9);
        numbers.add(1);

        // Элементы автоматически сортируются
        System.out.println("TreeSet: " + numbers);  // Вывод: [1, 2, 5, 9]

        // Проверка наличия элемента
        System.out.println("Contains 5: " + numbers.contains(5));  // Вывод: true

        // Удаление элемента
        numbers.remove(9);
        System.out.println("После удаления 9: " + numbers);  // Вывод: [1, 2, 5]
    }
}
```

По умолчанию `TreeSet` сортирует элементы в естественном порядке (если класс реализует интерфейс `Comparable`). Однако вы можете использовать собственный компаратор, чтобы задать кастомный порядок сортировки.

### 3. Операции с диапазонами

`TreeSet` предоставляет методы для работы с подмножествами и диапазонами:

- **`headSet()`** — возвращает все элементы меньше указанного.
- **`tailSet()`** — возвращает все элементы больше или равные указанному.
- **`subSet()`** — возвращает подмножество в диапазоне от одного элемента до другого.

#### Пример работы с диапазонами:
```java
import java.util.TreeSet;

public class TreeSetRangeExample {
    public static void main(String[] args) {
        TreeSet<Integer> numbers = new TreeSet<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        numbers.add(4);
        numbers.add(5);

        // Подмножество элементов меньше 4
        System.out.println("Elements less than 4: " + numbers.headSet(4));  // Вывод: [1, 2, 3]

        // Подмножество элементов больше или равных 3
        System.out.println("Elements greater than or equal to 3: " + numbers.tailSet(3));  // Вывод: [3, 4, 5]

        // Подмножество от 2 до 4 (не включая 4)
        System.out.println("SubSet from 2 to 4: " + numbers.subSet(2, 4));  // Вывод: [2, 3]
    }
}

```
### Методы

- `boolean add(E e)` - Добавляет указанный элемент в это множество, если его еще нет.
- `boolean addAll(Collection<? extends E> c)` - Добавляет все элементы из указанной коллекции в это множество.
- `E ceiling(E e)` - Возвращает наименьший элемент в этом множестве, больший или равный указанному элементу, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(20);
        set.add(30);
        set.add(40);
        set.add(50);

        // Использование метода ceiling
        Integer result1 = set.ceiling(25);
        System.out.println("Ceiling of 25: " + result1);
 // Ожидаемый результат: 30

        Integer result2 = set.ceiling(35);
        System.out.println("Ceiling of 35: " + result2);
 // Ожидаемый результат: 40

        Integer result3 = set.ceiling(55);
        System.out.println("Ceiling of 55: " + result3);
 // Ожидаемый результат: null
    }
}
```

- `void clear()` - Удаляет все элементы из этого множества.
- `Object clone()` - Возвращает поверхностную копию этого экземпляра TreeSet: сами элементы не клонируются.
- `Comparator<? super E> comparator()` - Возвращает компаратор, используемый для упорядочения элементов в этом множестве, или null, если это множество использует естественный порядок своих элементов.
- `boolean contains(Object o)` - Возвращает true, если это множество содержит указанный элемент.
- `Iterator<E> descendingIterator()` - Возвращает итератор по элементам этого множества в обратном порядке.
- `NavigableSet<E> descendingSet()` - Возвращает представление элементов этого множества в обратном порядке.
- `E first()` - Возвращает первый (наименьший) элемент, находящийся в данный момент в этом множестве.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода first
        Integer firstElement = set.first();
        System.out.println("First element: " + firstElement);
 // Ожидаемый результат: 5
    }
}
```

- `E floor(E e)` - Возвращает наибольший элемент в этом множестве, меньший или равный указанному элементу, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода floor
        Integer floorElement = set.floor(12);
        System.out.println("Floor element: " + floorElement); // Ожидаемый результат: 10
    }
}
```

- `SortedSet<E> headSet(E toElement)` - Возвращает представление части этого множества, элементы которой строго меньше toElement.

```Java
import java.util.TreeSet;
import java.util.SortedSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода headSet
        SortedSet<Integer> headSet = set.headSet(15);
        System.out.println("Head set: " + headSet);
 // Ожидаемый результат: [5, 10]
    }
}
```

- `NavigableSet<E> headSet(E toElement, boolean inclusive)` - Возвращает представление части этого множества, элементы которой меньше (или равны, если inclusive равен true) toElement.

```Java
import java.util.TreeSet;
import java.util.NavigableSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода headSet
        NavigableSet<Integer> headSet = set.headSet(15, true);
        System.out.println("Head set: " + headSet);
 // Ожидаемый результат: [5, 10, 15]
    }
}
```

- `E higher(E e)` - Возвращает наименьший элемент в этом множестве, строго больший указанного элемента, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода higher
        Integer higherElement = set.higher(10);
        System.out.println("Higher element: " + higherElement);
 // Ожидаемый результат: 15
    }
}
```

- `boolean isEmpty()` - Возвращает true, если это множество не содержит элементов.
- `Iterator<E> iterator()` - Возвращает итератор по элементам этого множества в возрастающем порядке.
- `E last()` - Возвращает последний (наивысший) элемент, находящийся в данный момент в этом множестве.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода last
        Integer lastElement = set.last();
        System.out.println("Last element: " + lastElement);
 // Ожидаемый результат: 20
    }
}
```

- `E lower(E e)` - Возвращает наибольший элемент в этом множестве, строго меньший указанного элемента, или null, если такого элемента нет.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода lower
        Integer lowerElement = set.lower(15);
        System.out.println("Lower element: " + lowerElement); 
// Ожидаемый результат: 10
    }
}
```

- `E pollFirst()` - Извлекает и удаляет первый (наименьший) элемент, или возвращает null, если это множество пусто.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода pollFirst
        Integer firstElement = set.pollFirst();
        System.out.println("First element: " + firstElement); 
// Ожидаемый результат: 5
        System.out.println("Set after removing first element: " + set);
 // Ожидаемый результат: [10, 15, 20]
    }
}
```

- `E pollLast()` - Извлекает и удаляет последний (наивысший) элемент, или возвращает null, если это множество пусто.

```Java
import java.util.TreeSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);

        // Использование метода pollLast
        Integer lastElement = set.pollLast();
        System.out.println("Last element: " + lastElement); // Ожидаемый результат: 20
        System.out.println("Set after removing last element: " + set); // Ожидаемый результат: [5, 10, 15]
    }
}
```

- `boolean remove(Object o)` - Удаляет указанный элемент из этого множества, если он присутствует.
- `int size()` - Возвращает количество элементов в этом множестве.
- `Spliterator<E> spliterator()` - Создает Spliterator, который относится к элементам этого множества и обладает поздней привязкой и стратегией быстрой остановки.
- `NavigableSet<E> subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)` - Возвращает представление части этого множества, элементы которой находятся в диапазоне от fromElement до toElement.

```Java
import java.util.TreeSet;
import java.util.NavigableSet;

public class Main {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(10);
        set.add(5);
        set.add(15);
        set.add(20);
        set.add(25);
        set.add(30);

        // Использование метода subSet
        NavigableSet<Integer> subset = set.subSet(10, true, 25, false);
        System.out.println("Subset: " + subset);
 // Ожидаемый результат: [10, 15, 20]
    }
}
```

- `SortedSet<E> subSet(E fromElement, E toElement)` - Возвращает представление части этого множества, элементы которой находятся в диапазоне от fromElement (включительно) до toElement (исключительно).

```Java
SortedSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9));
SortedSet<Integer> subset = set.subSet(3, 8);
System.out.println(subset); // выведет [3, 4, 5, 6, 7]
```

- `SortedSet<E> tailSet(E fromElement)` - Возвращает представление части этого множества, элементы которой больше или равны fromElement.

```Java
SortedSet<String> set = new TreeSet<>(Arrays.asList("apple", "banana", "cherry", "date", "elderberry"));
SortedSet<String> tailSet = set.tailSet("cherry");
System.out.println(tailSet); // выведет [cherry, date, elderberry]
```

- `NavigableSet<E> tailSet(E fromElement, boolean inclusive)` - Возвращает представление части этого множества, элементы которой больше (или равны, если inclusive равен true) fromElement.

```Java
NavigableSet<Integer> set = new TreeSet<>(Arrays.asList(1, 2, 3, 4, 5));
NavigableSet<Integer> tailSet = set.tailSet(3, true);
System.out.println(tailSet); // выведет [3, 4, 5]
```




![[images/Untitled 20 8.png|Untitled 20 8.png]]

- `Set` ==нельзя отсортировать, кроме реализации== `TreeSet`
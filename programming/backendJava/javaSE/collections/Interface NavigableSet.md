---
title: Interface NavigableSet
tags:
  - Collection
  - Set
related_topics: 
created: 2024-09-09 17:19
modified: 2024-09-10T13:50:05+03:00
questions: 
notes: 
links: 
---
![[images/Untitled 2 19.png|Untitled 2 19.png]]
### Методы interface NavigableSet\<E>

- `ceiling(E e)`: возвращает наименьший элемент в этом наборе, который больше или равен заданному элементу `e`, или `null`, если такого элемента нет.
- `descendingIterator()`: возвращает итератор по элементам в этом наборе в обратном порядке.
- `descendingSet()`: возвращает вид на элементы, содержащиеся в этом наборе в обратном порядке.
- `floor(E e)`: возвращает наибольший элемент в этом наборе, который меньше или равен заданному элементу `e`, или `null`, если такого элемента нет.
- `headSet(E toElement)`: возвращает вид на часть этого набора, элементы которой строго меньше `toElement`.
- `headSet(E toElement, boolean inclusive)`: возвращает вид на часть этого набора, элементы которой меньше (или равны, если `inclusive = true`) `toElement`.
- `higher(E e)`: возвращает наименьший элемент в этом наборе, который строго больше заданного элемента `e`, или `null`, если такого элемента нет.
- `iterator()`: возвращает итератор по элементам в этом наборе в возрастающем порядке.
- `lower(E e)`: возвращает наибольший элемент в этом наборе, который строго меньше заданного элемента `e`, или `null`, если такого элемента нет.
- `pollFirst()`: извлекает и удаляет первый (наименьший) элемент, или возвращает `null`, если этот набор пуст.
- `pollLast()`: извлекает и удаляет последний (наибольший) элемент, или возвращает `null`, если этот набор пуст.
- `subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)`: возвращает вид на часть этого набора, элементы которой находятся в диапазоне от `fromElement` до `toElement`.
- `subSet(E fromElement, E toElement)`: возвращает вид на часть этого набора, элементы которой находятся в диапазоне от `fromElement` (включительно) до `toElement` (исключительно).
- `tailSet(E fromElement)`: возвращает вид на часть этого набора, элементы которой больше или равны `fromElement`.
- `tailSet(E fromElement, boolean inclusive)`: возвращает вид на часть этого набора, элементы которой больше (или равны, если `inclusive = true`) `fromElement`.
- `comparator()`: возвращает компаратор, используемый для упорядочивания элементов в этом наборе, или `null`, если набор использует естественное упорядочение своих элементов.

Classes: [AbstractSet](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSet.html), [ConcurrentHashMap.KeySetView](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.KeySetView.html), [ConcurrentSkipListSet](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentSkipListSet.html), [CopyOnWriteArraySet](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArraySet.html), [EnumSet](https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html), [HashSet](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html), [JobStateReasons](https://docs.oracle.com/javase/8/docs/api/javax/print/attribute/standard/JobStateReasons.html), [LinkedHashSet](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashSet.html), [TreeSet](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)


```java
import java.util.NavigableSet;
import java.util.TreeSet;

public class NavigableSetExample {
    public static void main(String[] args) {
        NavigableSet<Integer> numbers = new TreeSet<>();
        
        numbers.add(10);
        numbers.add(20);
        numbers.add(30);
        numbers.add(40);
        numbers.add(50);

        System.out.println("Original Set: " + numbers);  // [10, 20, 30, 40, 50]

        // Поиск элементов относительно значения 25
        System.out.println("Lower than 25: " + numbers.lower(25));  // 20
        System.out.println("Floor of 25: " + numbers.floor(25));    // 20
        System.out.println("Ceiling of 25: " + numbers.ceiling(25)); // 30
        System.out.println("Higher than 25: " + numbers.higher(25)); // 30

        // Получение подмножества
        NavigableSet<Integer> subset = numbers.subSet(20, true, 40, true);
        System.out.println("Subset from 20 to 40: " + subset); // [20, 30, 40]

        // Извлечение и удаление первого и последнего элемента
        System.out.println("First element: " + numbers.pollFirst());  // 10
        System.out.println("Last element: " + numbers.pollLast());    // 50
        System.out.println("After polling first and last: " + numbers); // [20, 30, 40]
    }
}

```

#### Пример 2: Использование `descendingSet()`

```java
import java.util.NavigableSet;
import java.util.TreeSet;

public class DescendingSetExample {
    public static void main(String[] args) {
        NavigableSet<String> names = new TreeSet<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");
        names.add("David");

        System.out.println("Original Set: " + names);  // [Alice, Bob, Charlie, David]

        // Получение множества с элементами в обратном порядке
        NavigableSet<String> descendingNames = names.descendingSet();
        System.out.println("Descending Set: " + descendingNames);  // [David, Charlie, Bob, Alice]
    }
}

```

#### Пример 3: Использование `subSet()` для диапазонов

```java
import java.util.NavigableSet;
import java.util.TreeSet;

public class SubSetExample {
    public static void main(String[] args) {
        NavigableSet<Integer> numbers = new TreeSet<>();
        numbers.add(5);
        numbers.add(10);
        numbers.add(15);
        numbers.add(20);
        numbers.add(25);

        // Получение подмножества в диапазоне от 10 (включительно) до 20 (не включительно)
        NavigableSet<Integer> rangeSet = numbers.subSet(10, true, 20, false);
        System.out.println("Subset from 10 to 20: " + rangeSet);  // [10, 15]

        // Попробуем добавить элемент, который не входит в диапазон
        try {
            rangeSet.add(25);  // Выбросит IllegalArgumentException
        } catch (IllegalArgumentException e) {
            System.out.println("Cannot add 25 to subset: " + e.getMessage());
        }
    }
}

```

#### Пример 4: Использование `floor()` и `ceiling()`

```java
import java.util.NavigableSet;
import java.util.TreeSet;

public class FloorCeilingExample {
    public static void main(String[] args) {
        NavigableSet<Integer> numbers = new TreeSet<>();
        numbers.add(100);
        numbers.add(200);
        numbers.add(300);
        numbers.add(400);
        numbers.add(500);

        // Поиск ближайших элементов относительно значения
        System.out.println("Floor of 350: " + numbers.floor(350));  // 300
        System.out.println("Ceiling of 350: " + numbers.ceiling(350));  // 400
    }
}

```


### Плюсы и минусы `NavigableSet`

#### Плюсы:

1. **Отсортированные данные**:
    
    - Элементы всегда хранятся в отсортированном порядке.
    - Удобно для работы с диапазонами, подмножествами и для нахождения ближайших элементов относительно указанного значения.
2. **Дополнительные методы**:
    
    - Методы для поиска ближайших элементов (`lower()`, `floor()`, `ceiling()`, `higher()`), а также для работы с подмножествами делают `NavigableSet` гибким инструментом для задач поиска и фильтрации.
3. **Обратный порядок**:
    
    - Возможность получать множество в обратном порядке с помощью метода `descendingSet()`, что может быть полезно для ряда задач.
4. **Безопасные операции с подмножествами**:
    
    - Методы `subSet()`, `headSet()`, и `tailSet()` позволяют получить части множества, обеспечивая при этом строгие проверки на принадлежность элементов.

#### Минусы:

1. **Медленная производительность по сравнению с `HashSet`**:
    
    - Поскольку `NavigableSet` реализуется на основе сбалансированного дерева (`TreeSet`), операции добавления, удаления и поиска занимают `O(log n)` времени. Это медленнее, чем у `HashSet`, где эти операции выполняются за `O(1)`.
2. **Невозможность хранения `null`**:
    
    - В отличие от некоторых других коллекций, `NavigableSet` не позволяет хранить `null`, так как это нарушает логику сравнения элементов.
3. **Память и накладные расходы**:
    
    - Из-за использования структуры дерева (красно-черного дерева) `TreeSet` использует больше памяти, чем, например, хешированные структуры, такие как `HashSet`.
4. **Неэффективно для частого обновления**:
    
    - Если коллекция часто модифицируется (например, происходит много операций добавления и удаления), `NavigableSet` может быть менее эффективен по сравнению с другими коллекциями, так как каждая операция требует поддержания сбалансированного дерева.

---

### Когда применять `NavigableSet`?

1. **Когда нужна сортировка**:
    
    - Если требуется хранить элементы в отсортированном порядке и иметь возможность быстро находить ближайшие элементы или работать с диапазонами значений.
2. **Когда важны операции с подмножествами**:
    
    - Если нужно работать с подмножествами данных, задавать границы (например, получать элементы между двумя значениями).
3. **Для задач фильтрации**:
    
    - Когда нужно быстро находить наименьший или наибольший элемент относительно заданного значения или работать с диапазонами элементов.

---

### Когда **не** стоит применять `NavigableSet`?

1. **Когда важна высокая производительность**:
    
    - Если вам нужны быстрые операции вставки и удаления (например, для огромных коллекций), лучше использовать `HashSet`, так как его операции работают за `O(1)`.
2. **Если не требуется сортировка**:
    
    - Если порядок элементов не важен, использование `NavigableSet` будет избыточным. `HashSet` или другие несортированные коллекции будут работать быстрее и эффективнее.
3. **Когда нужно хранить `null`**:
    
    - Если коллекция должна поддерживать `null` значения, лучше выбрать коллекции, которые это допускают (например, `ArrayList`, `HashSet` и т.д.).
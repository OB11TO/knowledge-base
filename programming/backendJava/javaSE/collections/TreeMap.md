---
title: TreeMap
tags:
  - Collection
  - Map
related_topics: 
created: 2024-09-09 18:15
modified: 2024-09-10T13:58:08+03:00
questions: 
notes: 
links: 
---
### class TreeMap<K,V>

All Implemented Interfaces:[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)<K,V>, [NavigableMap](https://docs.oracle.com/javase/8/docs/api/java/util/NavigableMap.html)<K,V>, [SortedMap](https://docs.oracle.com/javase/8/docs/api/java/util/SortedMap.html)<K,V>

TreeMap в Java представляет собой отображение, которое хранит ключи в отсортированном порядке. Основным преимуществом TreeMap является то, что она хранит ключи в отсортированном порядке, что упрощает поиск и сравнение элементов. Однако у TreeMap есть и свои недостатки.

<mark class="hltr-orange">Плюсы</mark> TreeMap:

- Отображение хранит ключи в отсортированном порядке, что упрощает поиск и сравнение элементов.
- TreeMap позволяет использовать пользовательские объекты в качестве ключей, если они реализуют интерфейс Comparable или если задан компаратор.
- TreeMap предоставляет множество методов для работы с отсортированными данными, таких как методы для получения подмножеств отображения или для получения первого или последнего ключа в отсортированном порядке.

<mark class="hltr-orange">Минусы</mark> TreeMap:

- TreeMap может быть несколько медленнее, чем другие типы отображений, такие как HashMap, из-за необходимости сортировки ключей при каждом изменении отображения.
- Если пользовательские объекты используются в качестве ключей и не реализуют интерфейс Comparable или не задан компаратор, то при попытке использования TreeMap может возникнуть ошибка ClassCastException.
- TreeMap не является потокобезопасной, поэтому требует дополнительной синхронизации в многопоточной среде.

### Методы

- [`Map.Entry`](https://docs.oracle.com/javase/8/docs/api/java/util/Map.Entry.html)`<`[`K`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)`,`[`V`](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)`> Map.Entry<K,V> ceilingEntry(K key)` - возвращает отображение ключ-значение, связанное с наименьшим ключом, который больше или равен заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> entry = treeMap.ceilingEntry(4);
        System.out.println(entry); // Вывод: 5=Orange
    }
}
```

- `K ceilingKey(K key)` - возвращает наименьший ключ, который больше или равен заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Integer ceilingKey = treeMap.ceilingKey(4);
        System.out.println(ceilingKey); // Вывод: 5
    }
}
```

- `void clear()` - удаляет все отображения из данного отображения.
- `Object clone()` - возвращает поверхностную копию этого экземпляра TreeMap.
- `Comparator<? super K> comparator()` - возвращает компаратор, используемый для упорядочивания ключей в этом отображении, или null, если это отображение использует естественное упорядочивание своих ключей.
- `boolean containsKey(Object key)` - возвращает true, если в этом отображении содержится отображение для указанного ключа.
- `boolean containsValue(Object value)` - возвращает true, если это отображение содержит один или несколько ключей, связанных с указанным значением.
- `NavigableSet<K> descendingKeySet()` - возвращает представление упорядоченного множества ключей в обратном порядке, содержащегося в этом отображении.

```Java
import java.util.TreeMap;
import java.util.NavigableSet;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        NavigableSet<Integer> descendingKeySet = treeMap.descendingKeySet();
        System.out.println(descendingKeySet); // Вывод: [7, 5, 3, 1]
    }
}
```

- `NavigableMap<K,V> descendingMap()` - возвращает обратный порядок представления отображения для отображений, содержащихся в этом отображении.

```Java
import java.util.TreeMap;
import java.util.NavigableMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        NavigableMap<Integer, String> descendingMap = treeMap.descendingMap();
        for (Map.Entry<Integer, String> entry : descendingMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}

7 - Mango
5 - Orange
3 - Banana
1 - Apple
```

- `Set<Map.Entry<K,V>> entrySet()` - возвращает представление отображения в виде набора отображений, содержащихся в данном отображении.

```Java
import java.util.TreeMap;
import java.util.Map;
import java.util.Set;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Set<Map.Entry<Integer, String>> entrySet = treeMap.entrySet();
        for (Map.Entry<Integer, String> entry : entrySet) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}
1 - Apple
3 - Banana
5 - Orange
7 - Mango
```

- `Map.Entry<K,V> firstEntry()` - возвращает отображение ключ-значение, связанное с наименьшим ключом в этом отображении, или null, если отображение пусто.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Map.Entry<Integer, String> firstEntry = treeMap.firstEntry();
        System.out.println(firstEntry.getKey() + " - " + firstEntry.getValue()); // Вывод: 1 - Banana
    }
}

1 - Banana
```

- K `firstKey()` - возвращает первый (наименьший) ключ в этой карте.
- Map.Entry<K,V> `floorEntry(K key)` - возвращает отображение ключ-значение, связанное с наибольшим ключом, меньшим или равным заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> floorEntry = treeMap.floorEntry(4);
        System.out.println(floorEntry.getKey() + " - " + floorEntry.getValue()); // Вывод: 3 - Banana
    }
}

3 - Banana
```

- K `floorKey(K key)` - возвращает наибольший ключ, меньший или равный заданному ключу, или null, если такого ключа нет.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Integer floorKey = treeMap.floorKey(4);
        System.out.println(floorKey); // Вывод: 3
    }
}
```

- void `forEach(BiConsumer<? super K,? super V> action)` - выполняет заданное действие для каждой записи в этой карте, пока все записи не будут обработаны или действие не вызовет исключение.
- V `get(Object key)` - возвращает значение, сопоставленное с указанным ключом, или null, если в этой карте нет отображения для ключа.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        String value = treeMap.get(3);
        System.out.println(value); // Вывод: Banana
    }
}
```

- SortedMap<K,V> `headMap(K toKey)` - возвращает вид части этой карты, ключи которой строго меньше toKey.

```Java
import java.util.TreeMap;
import java.util.SortedMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        SortedMap<Integer, String> headMap = treeMap.headMap(5);
        for (Map.Entry<Integer, String> entry : headMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}

1 - Apple
3 - Banana
```

- NavigableMap<K,V> `headMap(K toKey, boolean inclusive)` - возвращает вид части этой карты, ключи которой меньше (или равны, если inclusive равно true) toKey.

```Java
import java.util.TreeMap;
import java.util.NavigableMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        NavigableMap<Integer, String> headMapInclusive = treeMap.headMap(5, true);
        System.out.println("Inclusive:");
        for (Map.Entry<Integer, String> entry : headMapInclusive.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
Inclusive:
1 - Apple
3 - Banana
5 - Orange
        NavigableMap<Integer, String> headMapExclusive = treeMap.headMap(5, false);
        System.out.println("Exclusive:");
        for (Map.Entry<Integer, String> entry : headMapExclusive.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}
Exclusive:
1 - Apple
3 - Banana
```

- Map.Entry<K,V> `higherEntry(K key)` - возвращает отображение ключ-значение, связанное с наименьшим ключом, строго большим заданного ключа, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> higherEntry = treeMap.higherEntry(4);
        System.out.println(higherEntry.getKey() + " - " + higherEntry.getValue()); // Вывод: 5 - Orange
    }
}

5 - Orange
```

- K `higherKey(K key)` - возвращает наименьший ключ, строго больший заданного ключа, или null, если такого ключа нет.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Integer higherKey = treeMap.higherKey(4);
        System.out.println(higherKey); // Вывод: 5
    }
}
```

- Set<\K> `keySet()` - возвращает вид множества ключей, содержащихся в этой карте.
- Map.Entry<K,V> `lastEntry()` - возвращает отображение ключ-значение, связанное с наибольшим ключом в этой карте, или null, если карта пуста.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Map.Entry<Integer, String> lastEntry = treeMap.lastEntry();
        System.out.println(lastEntry.getKey() + " - " + lastEntry.getValue()); // Вывод: 3 - Apple
    }
}

3 - Apple
```

- K `lastKey()` - возвращает последний (наибольший) ключ в этой карте.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Integer lastKey = treeMap.lastKey();
        System.out.println(lastKey); // Вывод: 3
    }
}
```

- `Map.Entry<K,V> lowerEntry(K key)`: возвращает отображение ключ-значение, связанное с наибольшим ключом, строго меньшим, чем заданный ключ, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");

        Map.Entry<Integer, String> lowerEntry = treeMap.lowerEntry(4);
        System.out.println(lowerEntry.getKey() + " - " + lowerEntry.getValue()); // Вывод: 3 - Banana
    }
}

3 - Banana
```

- `K lowerKey(K key)`: возвращает наибольший ключ, строго меньший, чем заданный ключ, или null, если такого ключа нет.

```Java
import java.util.TreeMap;
import java.util.NavigableSet;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        NavigableSet<Integer> navigableKeySet = treeMap.navigableKeySet();
        for (Integer key : navigableKeySet) {
            System.out.println(key);
        }
    }
}
1
2
3
```

- `NavigableSet<K> navigableKeySet()`: возвращает представление навигируемого набора ключей, содержащихся в данном отображении.
- `Map.Entry<K,V> pollFirstEntry()`: удаляет и возвращает отображение ключ-значение, связанное с наименьшим ключом в этом отображении, или null, если отображение пусто.

```Java
import java.util.TreeMap;
import java.util.NavigableSet;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        NavigableSet<Integer> navigableKeySet = treeMap.navigableKeySet();
        for (Integer key : navigableKeySet) {
            System.out.println(key);
        }
    }
}

1
2
3
```

- `Map.Entry<K,V> pollLastEntry()`: удаляет и возвращает отображение ключ-значение, связанное с наибольшим ключом в этом отображении, или null, если отображение пусто.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(3, "Apple");
        treeMap.put(1, "Banana");
        treeMap.put(2, "Orange");

        Map.Entry<Integer, String> lastEntry = treeMap.pollLastEntry();
        if (lastEntry != null) {
            System.out.println(lastEntry.getKey() + " - " + lastEntry.getValue());
        } else {
            System.out.println("TreeMap is empty.");
        }
    }
}

3 - Apple
```

- `V put(K key, V value)`: связывает заданное значение с заданным ключом в этом отображении.
- `void putAll(Map<? extends K,? extends V> map)`: копирует все отображения из заданного отображения в данное отображение.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        // Создание и заполнение другого Map
        Map<Integer, String> otherMap = new TreeMap<>();
        otherMap.put(1, "Apple");
        otherMap.put(2, "Banana");
        otherMap.put(3, "Orange");

        // Добавление всех записей из otherMap в treeMap
        treeMap.putAll(otherMap);

        // Вывод treeMap
        for (Map.Entry<Integer, String> entry : treeMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}
```

- `V remove(Object key)`: удаляет отображение ключ-значение для этого ключа из этого отображения, если он присутствует.
- `V replace(K key, V value)`: заменяет запись для указанного ключа только в том случае, если он сопоставлен с каким-то значением.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);

        Integer previousValue = treeMap.replace("Banana", 7);
        System.out.println("Previous value: " + previousValue); // Вывод: Previous value: 5

        Integer updatedValue = treeMap.get("Banana");
        System.out.println("Updated value: " + updatedValue); // Вывод: Updated value: 7
    }
}
```

- `boolean replace(K key, V oldValue, V newValue)`: заменяет запись для указанного ключа только в том случае, если он в данный момент сопоставлен с указанным значением.

```Java
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);

        boolean replaced = treeMap.replace("Banana", 5, 7);
        System.out.println("Replaced: " + replaced); // Вывод: Replaced: true

        Integer updatedValue = treeMap.get("Banana");
        System.out.println("Updated value: " + updatedValue); // Вывод: Updated value: 7

        boolean notReplaced = treeMap.replace("Banana", 5, 9);
        System.out.println("Replaced: " + notReplaced); // Вывод: Replaced: false

        Integer unchangedValue = treeMap.get("Banana");
        System.out.println("Unchanged value: " + unchangedValue); // Вывод: Unchanged value: 7
    }
}
```

- `void replaceAll(BiFunction<? super K,? super V,? extends V> function)`: заменяет значение каждой записи результатом вызова данной функции на эту запись, пока все записи не будут обработаны или пока функция не выбросит исключение.

```Java
import java.util.TreeMap;
import java.util.function.BiFunction;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);

        BiFunction<String, Integer, Integer> function = (key, value) -> value * 2;

        treeMap.replaceAll(function);

        for (String key : treeMap.keySet()) {
            System.out.println(key + " - " + treeMap.get(key));
        }
    }
}

Apple - 6
Banana - 10
Orange - 4
```

- `int size()`: возвращает количество записей ключ-значение в этом отображении.
- `NavigableMap<K,V> subMap(K fromKey, boolean fromInclusive, K toKey, boolean toInclusive)`: возвращает представление части этого отображения, ключи которого находятся в диапазоне от fromKey до toKey.

```Java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        // Получение подмножества от ключа 3 (включительно) до ключа 7 (исключительно)
        NavigableMap<Integer, String> subMap = treeMap.subMap(3, true, 7, false);

        // Вывод записей подмножества
        for (Map.Entry<Integer, String> entry : subMap.entrySet()) {
            System.out.println(entry.getKey() + " - " + entry.getValue());
        }
    }
}

3 - Banana
5 - Orange
Обратите внимание, что подмножество содержит записи
с ключами 3 и 5, включая их значения.
Запись с ключом 7 не включается в подмножество 
из-за параметра toInclusive, который установлен в false.
```

- `SortedMap<K,V> subMap(K fromKey, K toKey)`: возвращает представление части этого отображения, ключи которого находятся в диапазоне от fromKey (включительно) до toKey (исключительно).

```Java
import java.util.TreeMap;
import java.util.SortedMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<String, Integer> treeMap = new TreeMap<>();

        treeMap.put("Apple", 3);
        treeMap.put("Banana", 5);
        treeMap.put("Orange", 2);
        treeMap.put("Mango", 4);
        treeMap.put("Pineapple", 1);

        // Получение подмножества от ключа "Apple" до ключа "Mango"
        SortedMap<String, Integer> subMap = treeMap.subMap("Apple", "Mango");

        // Вывод записей подмножества
        for (String key : subMap.keySet()) {
            System.out.println(key + " - " + subMap.get(key));
        }
    }
}

Apple - 3
Banana - 5
Mango - 4
```

- `SortedMap<K,V> tailMap(K fromKey)`: возвращает подмножество `SortedMap`, содержащее все записи с ключами, начиная от `fromKey` и до конца `TreeMap`. Подмножество включает все записи с ключами, большими или равными `fromKey`.

```Java
import java.util.TreeMap;
import java.util.SortedMap;

public class TreeMapExample {
    public static void main(String[] args) {
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        treeMap.put(1, "Apple");
        treeMap.put(3, "Banana");
        treeMap.put(5, "Orange");
        treeMap.put(7, "Mango");
        treeMap.put(9, "Pineapple");

        // Получение подмножества от ключа 5 (включительно) до конца TreeMap
        SortedMap<Integer, String> subMap = treeMap.tailMap(5);

        // Вывод записей подмножества
        for (Integer key : subMap.keySet()) {
            System.out.println(key + " - " + subMap.get(key));
        }
    }
}

5 - Orange
7 - Mango
9 - Pineapple
```


![[images/Untitled 17 9.png|Untitled 17 9.png]]

![[images/Untitled 18 8.png|Untitled 18 8.png]]

![[images/Untitled 19 8.png|Untitled 19 8.png]]

- ==ПОСЛЕ КАЖДОЙ ВСТАВКИ ПРОИСХОДИТ ПРОВЕРКА НА БАЛАНСИРОВКУ==



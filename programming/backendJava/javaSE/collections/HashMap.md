---
title: HashMap
tags:
  - Collection
  - Map
related_topics: 
created: 2024-09-09 18:09
modified: 2024-09-11T15:27:43+03:00
questions: 
notes: 
links: 
---
### class HashMap<K,V>

В основе HashMap лежит массив(элементы массива часто называют бакетами, (то есть корзина). Элементами данного массива являются структуры LinkedList. Данные структуры LinkedList заполняются элементами, которые мы добавляем в HaashMap.


<mark class="hltr-orange">Плюсы</mark>:

- Быстрый доступ к элементам по ключу. `HashMap` использует хеш-таблицу, что позволяет выполнять операции `get` и `put` за время O(1) в большинстве случаев.
- Гибкость в использовании. `HashMap` позволяет хранить любые объекты в качестве ключей и значений, что делает его очень гибким при использовании.

<mark class="hltr-orange">Минусы</mark>:

- Отсутствие гарантии порядка элементов. `HashMap` не гарантирует порядок элементов в хранилище, что может быть проблемой, если порядок элементов имеет значение для приложения.
- Низкая производительность в случае коллизий. При возникновении коллизий `HashMap` необходимо выполнить дополнительную работу, чтобы разрешить коллизию, что может привести к снижению производительности.
- Использование большого объема памяти. `HashMap` использует хеш-таблицу, которая может занимать больше памяти, чем другие структуры данных. Кроме того, размер хеш-таблицы должен быть достаточно большим, чтобы уменьшить вероятность коллизий, что может привести к дополнительному использованию памяти.
- Не подходит для упорядоченных данных. `HashMap` не является подходящим выбором, если нужно хранить данные в упорядоченном виде, поскольку он не гарантирует порядок элементов.

### Методы

- `void clear()`Удаляет все отображения из данной карты.

```Java
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        // Создаем экземпляр HashMap
        HashMap<String, Integer> hashMap = new HashMap<>();

        // Добавляем некоторые элементы в HashMap
        hashMap.put("A", 1);
        hashMap.put("B", 2);
        hashMap.put("C", 3);

        System.out.println("Исходная HashMap: " + hashMap);

        // Очищаем HashMap с помощью метода clear()
        hashMap.clear();

        System.out.println("После очистки HashMap: " + hashMap);
    }
}
Исходная HashMap: {A=1, B=2, C=3}
После очистки HashMap: {}
```

- `Object clone()`Возвращает поверхностную копию этого экземпляра HashMap: ключи и значения не клонируются.
- `V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)`Пытается вычислить отображение для указанного ключа и его текущего сопоставленного значения (или null, если нет текущего отображения).

```Java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);

BiFunction<String, Integer, Integer> func = (key, value) -> value + 1;
map.compute("a", func); // обновит значение для ключа "a" на 2
map.compute("c", func); // не изменит отображение, так как ключа "c" нет в отображении
```

- `V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)`Если указанный ключ еще не связан со значением (или связан с null), пытается вычислить его значение с помощью данной функции отображения и вводит его в эту карту, если значение не null.
-  **Назначение**: Этот метод используется для вычисления и добавления значения в `Map`, если для указанного ключа **нет значения** (значение отсутствует или `null`).
- **Логика работы**: Если значение для ключа уже существует, метод **ничего не делает**. Если значение отсутствует, он вычисляет его с помощью переданной функции и добавляет его в `Map`.
```java
Map<String, Integer> map = new HashMap<>();
map.put("key1", 1);

// Вычисляем значение для "key2", т.к. его нет в карте
map.computeIfAbsent("key2", key -> key.length());

System.out.println(map);  // {key1=1, key2=4}

```
- `V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)`Если значение для указанного ключа присутствует и не равно null, пытается вычислить новое отображение с заданным ключом и его текущим отображенным значением.
- -= **Назначение**: Этот метод используется для обновления значения для существующего ключа. Он применяется только тогда, когда для ключа **уже есть значение**.
- **Логика работы**: Если значение для ключа присутствует, он обновляет его, используя переданную функцию. Если ключ отсутствует или его значение `null`, метод **ничего не делает**.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
map.computeIfPresent("one", (key, value) -> value + 1);
System.out.println(map); // Output: {one=2, two=2}

map.computeIfPresent("three", (key, value) -> value + 1);
System.out.println(map); // Output: {one=2, two=2}
```

- `boolean containsKey(Object key)`Возвращает true, если в этой карте присутствует отображение для указанного ключа.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.containsKey("one")); // Output: true
System.out.println(map.containsKey("three")); // Output: false
```

- `boolean containsValue(Object value)`Возвращает true, если эта карта отображает один или несколько ключей на указанное значение.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.containsValue(1)); // Output: true
System.out.println(map.containsValue(3)); // Output: false
```

- `Set<Map.Entry<K,V>> entrySet()`Возвращает представление набора отображений, содержащихся в этой карте.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);
map.put("C", 3);

Set<Map.Entry<String, Integer>> entrySet = map.entrySet();

for (Map.Entry<String, Integer> entry : entrySet) {
    String key = entry.getKey();
    int value = entry.getValue();
    System.out.println(key + " -> " + value);
}

ВЫВОД:
A -> 1
B -> 2
C -> 3
```

- `void forEach(BiConsumer<? super K,? super V> action)`  
    Выполняет заданное действие для каждой записи в этой карте, пока все записи не будут обработаны или пока действие не вызовет исключение.  
    

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
map.forEach((key, value) -> System.out.println(key + " = " + value));
```

- `V get(Object key)`Возвращает значение, к которому отображен указанный ключ, или null, если в этой карте отсутствует отображение для ключа.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.get("one")); // Output: 1
System.out.println(map.get("three")); // Output: null
```

- `V getOrDefault(Object key, V defaultValue)`Возвращает значение, к которому отображен указанный ключ, или defaultValue, если в этой карте отсутствует отображение для ключа.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.getOrDefault("one", 0)); // Output: 1
System.out.println(map.getOrDefault("three", 0)); // Output: 0
```

- `boolean isEmpty()`Возвращает true, если в этой карте нет отображений ключ-значение.
- `Set<K> keySet()`Возвращает представление набора ключей, содержащихся в этой карте.`V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)`Если указанный ключ еще не связан со значением или связан с null, связывает его с заданным ненулевым значением.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("orange", 3);

Set<String> keys = map.keySet();
System.out.println("Keys: " + keys);

Keys: [orange, apple, banana]
```

- ``[`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`` [`merge`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html#merge-K-V-java.util.function.BiFunction-)`(`[`K`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html) `key,` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html) `value,` [`BiFunction`](https://docs.oracle.com/javase/8/docs/api/java/util/function/BiFunction.html)`<? super` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`,? super` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`,? extends` [`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`> remappingFunction)` позволяет объединить значение, связанное с указанным ключом, и новое значение, используя заданную функцию для выполнения операции слияния. В частности, если ключ уже присутствует в отображении, то новое значение будет добавлено к текущему значению с помощью заданной функции слияния. Если ключ не существует, то новое значение будет добавлено в отображение.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);

// добавляем новое отображение с помощью merge()
map.merge("three", 3, (oldVal, newVal) -> oldVal + newVal);
System.out.println(map); // вывод: {one=1, two=2, three=3}

// обновляем существующее отображение с помощью merge()
map.merge("one", 5, (oldVal, newVal) -> oldVal + newVal);
System.out.println(map); // вывод: {one=6, two=2, three=3}
```

- `V put(K key, V value)`Связывает указанное значение с указанным ключом в этой карте.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
map.put("three", 3);
System.out.println(map); // Output: {one=1, two=2, three=3}
```

- `void putAll(Map<? extends K, ? extends V> m)` - копирует все отображения из указанного отображения в данное отображение.

```Java
HashMap<String, Integer> map1 = new HashMap<>();
map1.put("a", 1);
map1.put("b", 2);

HashMap<String, Integer> map2 = new HashMap<>();
map2.put("c", 3);
map2.put("d", 4);

map1.putAll(map2);
ВЫВОД {a=1, b=2, c=3, d=4}
```

- `V putIfAbsent(K key, V value)` - если указанный ключ еще не связан с каким-либо значением (или связан с `null`), то связывает его с указанным значением и возвращает `null`. Если ключ уже связан с каким-либо значением, то возвращает текущее значение.

```Java
import java.util.HashMap;
import java.util.Map;

public class Main {
  public static void main(String[] args) {
    Map<String, String> map = new HashMap<>();
    map.put("key1", "value1");

    String oldValue = map.putIfAbsent("key1", "new_value");
    System.out.println("Old value for key1: " + oldValue);
 // Output: Old value for key1: value1

    String newValue = map.putIfAbsent("key2", "value2");
    System.out.println("New value for key2: " + newValue);
 // Output: New value for key2: value2
  }
}
```

- `V remove(Object key)` - удаляет отображение для указанного ключа из этого отображения, если оно присутствует.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("orange", 2);
map.remove("apple"); // удаляем отображение для ключа "apple"
System.out.println(map); // выводит: {orange=2}
```

- `boolean remove(Object key, Object value)` - удаляет запись для указанного ключа только в том случае, если он в настоящее время связан с указанным значением.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("orange", 2);
map.remove("apple", 1); // не удаляем, так как значение для ключа "apple" не равно 1
map.remove("orange", 2); // удаляем, так как значение для ключа "orange" равно 2
System.out.println(map); // выводит: {apple=1}
```

- `V replace(K key, V value)` - заменяет запись для указанного ключа только в том случае, если он в настоящее время связан с каким-либо значением, и возвращает предыдущее значение, связанное с ключом.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
map.put("banana", 5);

int oldValue = map.replace("apple", 20); // заменяем значение ключа "apple" на 20
System.out.println("Старое значение для ключа \"apple\": " + oldValue); // выведет "Старое значение для ключа "apple": 10"
System.out.println("Новое значение для ключа \"apple\": " + map.get("apple")); // выведет "Новое значение для ключа "apple": 20"
```

- `boolean replace(K key, V oldValue, V newValue)` - заменяет запись для указанного ключа только в том случае, если он в настоящее время связан с указанным значением, и возвращает `true`. В противном случае он ничего не делает и возвращает `false`.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
map.put("banana", 5);

boolean replaced = map.replace("apple", 10, 20); // заменяем значение ключа "apple" с 10 на 20
System.out.println("Значение ключа \"apple\" заменено? " + replaced); // выведет "Значение ключа "apple" заменено? true"
System.out.println("Новое значение для ключа \"apple\": " + map.get("apple")); // выведет "Новое значение для ключа "apple": 20"
```

- `void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)` - заменяет значение каждого отображения на результат вызова данной функции для этой записи, пока не будут обработаны все записи или пока функция не выдаст исключение.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 10);
map.put("banana", 5);

// увеличиваем значения каждого отображения на 5
map.replaceAll((key, value) -> value + 5);

System.out.println("Новое значение для ключа \"apple\": " + map.get("apple")); // выведет "Новое значение для ключа "apple": 15"
System.out.println("Новое значение для ключа \"banana\": " + map.get("banana")); // выведет "Новое значение для ключа "banana": 10"
```

- `int size()` - возвращает количество отображений в данном отображении.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("cherry", 3);

int size = map.size(); // size равен 3
```

- [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<`[`V`](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)`> Collection<V> values()` - возвращает коллекцию, содержащую все значения в данном отображении.

```Java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("cherry", 3);

Collection<Integer> values = map.values();
 // возвращает коллекцию [1, 2, 3]
```


![[images/Untitled 9 15.png|Untitled 9 15.png]]

![[images/Untitled 10 13.png|Untitled 10 13.png]]

![[images/Untitled 11 13.png|Untitled 11 13.png]]

![[images/Untitled 12 13.png|Untitled 12 13.png]]

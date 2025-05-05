---
title: LinkedHashSet
tags:
  - Collection
  - Set
related_topics: 
created: 2024-09-09 17:30
modified: 2024-09-10T12:42:52+03:00
questions: 
notes: 
links: 
---
### class <mark class="hltr-orange">LinkedHashSet</mark>\<E> extends HashSet<\E>

<mark class="hltr-orange">Плюсы</mark>:

1. Порядок вставки элементов: LinkedHashSet сохраняет порядок вставки элементов. При итерации по множеству элементы будут возвращаться в том порядке, в котором они были добавлены. Это полезно, когда требуется сохранить порядок элементов и выполнить операции на основе порядка вставки.
2. Уникальность элементов: Как и другие реализации Set, LinkedHashSet гарантирует уникальность элементов. Если вы пытаетесь добавить элемент, который уже присутствует в множестве, операция добавления будет проигнорирована.
3. Быстрая операция поиска: LinkedHashSet использует хэш-таблицу для быстрой операции поиска элементов. Поиск элемента выполняется со сложностью O(1) в среднем случае.
4. Потокобезопасность: Если LinkedHashSet используется в многопоточной среде, можно использовать блокировку или другие механизмы синхронизации для обеспечения потокобезопасности при доступе к LinkedHashSet.

<mark class="hltr-orange">Минусы:</mark>

1. Относительно большое потребление памяти: LinkedHashSet требует дополнительной памяти для хранения связанного списка, который поддерживает порядок вставки элементов. Это может привести к большему потреблению памяти по сравнению с другими реализациями Set.
2. Медленные операции вставки и удаления: Вставка и удаление элементов в LinkedHashSet выполняются медленнее по сравнению с другими реализациями Set, такими как HashSet. Это связано с необходимостью поддержки порядка вставки элементов и обновления связанного списка.

### Методы

Наследует все методы от наследников

```Java
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;

public class HashSetVsLinkedHashSetExample {
    public static void main(String[] args) {
        // Пример с HashSet
        Set<String> hashSet = new HashSet<>();

        hashSet.add("банан");
        hashSet.add("яблоко");
        hashSet.add("апельсин");
        hashSet.add("груша");
        hashSet.add("манго");

        System.out.println("HashSet:");
        for (String fruit : hashSet) {
            System.out.println(fruit);
        }

HashSet:
манго
банан
яблоко
груша
апельсин

        // Пример с LinkedHashSet
        Set<String> linkedHashSet = new LinkedHashSet<>();

        linkedHashSet.add("банан");
        linkedHashSet.add("яблоко");
        linkedHashSet.add("апельсин");
        linkedHashSet.add("груша");
        linkedHashSet.add("манго");

        System.out.println("\nLinkedHashSet:");
        for (String fruit : linkedHashSet) {
            System.out.println(fruit);
        }
    }
}

LinkedHashSet:
банан
яблоко
апельсин
груша
манго
```

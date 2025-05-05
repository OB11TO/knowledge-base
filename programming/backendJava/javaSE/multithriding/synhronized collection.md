---
title: synhronized collection
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:30
modified: 2024-09-11T12:36:12+03:00
questions: 
notes: 
links: 
---
### synhronized collection

- ==Захват монитора является трудоемкой операцией.== Занимает процессорное время и влияет на перфоманс приложение.
- ==Такое использовать только в крайней случае==. Когда уже есть список.
- `НУЖНО ИСПОЛЬЗОВАТЬ Cuncurrent`

![[images/Untitled 11 2.png|Untitled 11 2.png]]

В классе Collections существуют методы, которые возвращают синхронизированную обертку над коллекцией:

- `static <T> Collection<T> synchronizedCollection(Collection<T> c)` - возвращает синхронизированную (потокобезопасную) коллекцию, поддерживаемую указанной коллекцией.
- `static <T> List<T> synchronizedList(List<T> list)` - возвращает синхронизированный (потокобезопасный) список, поддерживаемый указанным списком.
- `static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)` - возвращает синхронизированную (thread-safe) map, которая поддерживается указанной map.
- `static <K,V> NavigableMap<K,V> synchronizedNavigableMap(NavigableMap<K,V> m)` - возвращает синхронизированную (thread-safe) NavigableMap, которая поддерживается указанной NavigableMap.
- `static <T> NavigableSet<T> synchronizedNavigableSet(NavigableSet<T> s)` - возвращает синхронизированную (thread-safe) NavigableSet, которая поддерживается указанным NavigableSet.
- `static <T> Set<T> synchronizedSet(Set<T> s)` - возвращает синхронизированный (thread-safe) set, который поддерживается указанным set.
- `static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m)` - возвращает синхронизированный (thread-safe) sorted map, который поддерживается указанным sorted map.
- `static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s)` - возвращает синхронизированный (thread-safe) sorted set, который поддерживается указанным sorted set.
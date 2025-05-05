---
title: Interface List
tags:
  - Collection
  - List
related_topics: []
created: 2024-09-09 16:51
modified: 2025-02-24T12:00:55+03:00
questions: 
notes: 
links: 
---

----
[[Vector and Stack]]
[[ArrayList]]
[[LinkedList]]

[[List job4j]]


---
## Interface List\<E>

[https://docs.oracle.com/javase/8/docs/api/java/util/List.html](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)

All Known Implementing Classes:

[AbstractList](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractList.html), [AbstractSequentialList](https://docs.oracle.com/javase/8/docs/api/java/util/AbstractSequentialList.html), [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html), [AttributeList](https://docs.oracle.com/javase/8/docs/api/javax/management/AttributeList.html), [CopyOnWriteArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CopyOnWriteArrayList.html), [LinkedList](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html), [RoleList](https://docs.oracle.com/javase/8/docs/api/javax/management/relation/RoleList.html), [RoleUnresolvedList](https://docs.oracle.com/javase/8/docs/api/javax/management/relation/RoleUnresolvedList.html), [Stack](https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html), [Vector](https://docs.oracle.com/javase/8/docs/api/java/util/Vector.html)

  ![[Pasted image 20240909165740.png]]
![[Pasted image 20240909165923.png]]
### Default methods

`void` [`replaceAll`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#replaceAll-java.util.function.UnaryOperator-)`(`[`UnaryOperator`](https://docs.oracle.com/javase/8/docs/api/java/util/function/UnaryOperator.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`> operator)` з<mark class="hltr-yellow">аменяет каждый элемент списка на результат применения оператора</mark> `UnaryOperator` к этому элементу.

```Java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.replaceAll(n -> n + 1);
```

  

`void` [`sort`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#sort-java.util.Comparator-)`(`[`Comparator`](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)`<? super` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`> c)` <mark class="hltr-yellow">сортирует с помощью переданного компаратора</mark>

```Java
employees.sort(new Comparator<Employee>() {
            @Override
            public int compare(Employee o1, Employee o2) {
                if(o1.getSalary() > o2.getSalary()){
                    return  1;
                }
                else if(o1.getSalary() < o2.getSalary()){
                    return -1;
                }
                return 0;
            }
        });
```

[`Spliterator`](https://docs.oracle.com/javase/8/docs/api/java/util/Spliterator.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`>` [`spliterator`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#spliterator--)`()` возвращает объект `Spliterator`, который позволяет эффективно разбить коллекцию на части, что может быть полезно при параллельной обработке.

`Spliterator` похож на `Iterator`, но добавляет поддержку разбиения на части. Метод `tryAdvance` позволяет обработать каждый элемент последовательно, а метод `trySplit` возвращает новый объект `Spliterator`, который содержит часть элементов, на которых можно параллельно выполнить операции.

Использование `Spliterator` позволяет эффективно распараллелить операции над большими коллекциями данных.
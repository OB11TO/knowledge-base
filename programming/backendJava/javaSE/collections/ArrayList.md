---
title: ArrayList
tags:
  - Collection
  - List
related_topics: 
created: 2024-09-09 17:01
modified: 2025-02-24T12:12:48+03:00
questions: 
notes: 
links: 
---

### ArrayList

<mark class="hltr-purple">Наследуется от AbstractList</mark>
![[Pasted image 20240909170402.png]]

All Implemented Interfaces:

[Serializable](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html), [Cloneable](https://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html), [Iterable](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)\<E>, [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)\<E>, [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)\<E>, [RandomAccess](https://docs.oracle.com/javase/8/docs/api/java/util/RandomAccess.html)

<mark class="hltr-orange">Основные особенности ArrayList:</mark>

- ArrayList <mark class="hltr-yellow">основан на массивах, но предоставляет динамическую размерность</mark>, что позволяет добавлять и удалять элементы в массиве в любое время.
- ArrayList может содержать объекты любого типа данных, включая объекты пользовательского класса.
- ArrayList автоматически увеличивает свой размер при добавлении новых элементов и уменьшает его при удалении элементов. Это обеспечивает гибкость при работе с массивом.
- ArrayList <mark class="hltr-yellow">не является потокобезопасным, поэтому если необходимо использовать ArrayList в многопоточной среде, необходимо использовать синхронизацию.</mark>

### Конструкторы

- [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ArrayList--)`()` создает ArrayList с <mark class="hltr-purple">Capacity равным 10</mark>

При добавлении элемента в ArrayList, в котором не хватает места под капотом создается новый массив с большей размерностью, это занимает время, поэтому лучше использовать конструктов с размерностью, если известно, сколько элементов будет храниться в списке.

- [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ArrayList-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<? extends` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> c)` конструктор, который создает новый экземпляр списка `ArrayList`, содержащий элементы из заданной коллекции.

```Java
List<String> list1 = new ArrayList<>(Arrays.asList("apple",
 "banana", "orange"));
List<String> list2 = new ArrayList<>(list1);
System.out.println(list2); // вывод: [apple, banana, orange]
```

- [`ArrayList`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#ArrayList-int-)`(int initialCapacity)`

```Java
ArrayList<DataType> list = new ArrayList<>(30);
```

### Методы

- `boolean` [`add`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#add-E-)`(`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) `e)` добавляет объект в лист

```Java
employees.add(new Employee("Vadim", "Konovalov"));
```

- `void` [`add`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#add-int-E-)`(int index,`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) `element)` добавляет на определенную позицию объект, смещая остальные дальше

```Java
employees.add(new Employee(1, "Vadim", "Konovalov"));
```

- `boolean` [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#addAll-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<? extends` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> c)` добавляет в лист значения из другой коллекции

```Java
List<Employee> list = new ArrayList<>(10);
        list.addAll(employees);

List<String> myList = new ArrayList<>();
myList.addAll(Arrays.asList("one", "two", "three"));
```

- `boolean` [`addAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#addAll-int-java.util.Collection-)`(int index,` [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<? extends` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> c)` добавляет коллекцию с определенной позиции, смещая объекты дальше

```Java
List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
list.addAll(2,list);//  [1, 2, 1, 2, 3, 4, 3, 4]
```

- `void` [`clear`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#clear--)`()` очищает лист

```Java
list.clear();
```

- `boolean` [`contains`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#contains-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` проверяет содержится ли значение в листе

```Java
List<Integer> list = new ArrayList<Integer>();
        list.add(1);
        list.add(2);
	      boolean isContains = list.contains(2); // true
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) [`get`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#get-int-)`(int index)` возвращает объект по индексу Метод может кинуть исключение класса IndexOutOfBoundsException, если будет выполнено условие index < 0 || index > size().

```Java
List<Integer> list = new ArrayList<Integer>();
        list.add(1);
        list.add(2);
        Integer value = list.get(1);  //2
```

- `int` [`indexOf`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#indexOf-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` возвращает позицию объекта

```Java
List<Integer> list = new ArrayList<>();
list.add(5);
list.add(10);
sout(list.indexOf(10));  //return 1
```

- `boolean` [`isEmpty`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#isEmpty--)`()` проверяет пустой список или нет

```Java
List<Integer> list = new ArrayList<Integer>();
        System.out.println(list.isEmpty()); // true
```

- `int` [`lastIndexOf`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#lastIndexOf-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` возвращает индекс последнего вхождения указанного объекта в список, или -1, если список не содержит указанного объекта.

```Java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 2, 5, 6, 2));
int index = list.lastIndexOf(2);
System.out.println(index); // Output: 7
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#remove-int-)`(int index)` удаляет из списка объект по индексу

```Java
employees.remove(1);
```

- `boolean` [`remove`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#remove-java.lang.Object-)`(`[`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html) `o)` удаляет объект из списка

```Java
Student student = new Student("Vadim");
employees.remove(student);
```

- `boolean` [`removeAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#removeAll-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<?> c)` удаляет все элементы содержащиеся в прееданной коллекции

```Java
List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            list.add(i);
        }

List<Integer> list2 = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            list2.add(i);
        }
list.removeAll(list2);
System.out.println(list); [5, 6, 7, 8, 9]//
```

- `boolean` [`removeIf`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#removeIf-java.util.function.Predicate-)`(`[`Predicate`](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html)`<? super` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> filter)` удаляет элементы в соответствии с условием

```Java
List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            list.add(i);
        }
        list.removeIf(x-> x % 2!=0);
        System.out.println(list); // [0, 2, 4, 6, 8]
```

- `void` [`replaceAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#replaceAll-java.util.function.UnaryOperator-)`(`[`UnaryOperator`](https://docs.oracle.com/javase/8/docs/api/java/util/function/UnaryOperator.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)`> operator)` применяет функцию с замены, используя функции переданную в качестве аргумента

```Java
List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.replaceAll(x->x*10);
        System.out.println(list); [10,20]
```

- `boolean` [`retainAll`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#retainAll-java.util.Collection-)`(`[`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)`<?> c)` оставляет в списке, только те элементы, которые содержатся в коллекци, переданную в качестве аргумента

```Java
List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
List<Integer> list2 = new ArrayList<>();
        list2.add(1);
        list2.add(3);
list.retainAll(list2);
System.out.println(list);  //1
```

- [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) [`set`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#set-int-E-)`(int index,` [`E`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html) `element)` устанавливает на позицию новый элемент, удаляя предыдущий

```Java
employees.set(1, new Employee("Vadim","Konovalov");
```

- `int` [`size`](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html#size--)`()` возвращает размер списка

```Java
for(int i = 0; i< employees.size(); i++{
doSomething();
}
```

- [`List`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`<`[`E`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)`>` [`subList`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#subList-int-int-)`(int fromIndex, int toIndex)` возвращает представление (view) списка, которое содержит элементы в диапазоне от индекса `fromIndex` (включительно) до индекса `toIndex` (исключительно).

```Java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
List<Integer> sublist = list.subList(1, 4); // sublist содержит элементы [2, 3, 4]
```

То есть, вызывая `list.subList(fromIndex, toIndex)`, вы  
получаете новый список, который является представлением части исходного  
списка. Изменения в этом списке будут отражаться на исходном списке и  
наоборот.  

```Java
sublist.set(1, 10); // sublist теперь содержит элементы [2, 10, 4]
System.out.println(list); // [1, 2, 10, 4, 5]
```

- [`Object`](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)`[]` [`toArray`](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#toArray--)`()`

```Java
Employee [] array = (Employee[]) employees.toArray();
```


![[images/Untitled 160.png|Untitled 160.png]]

------

<mark class="hltr-red">Также для создания и одновременного заполнения списка можно использовать фабричный метод of():</mark>

```java
List<E> of(E ... elements)
List<String> result = List.of("one", "two", "three");
```
 - метод возвращает список, в которые помещены список элементов elements типа E. Как мы видим метод принимает переменное количество аргументов (обозначается ...). Т.е. мы можем перечислять большое количество аргументов через запятую.
 - <mark class="hltr-yellow">Очень важно понимать, что метод возвращает неизменяемый список, т.е. вызвать метод add(), remove() и т.п. на такой коллекции не получится, будет сгенерировано исключение UnsupportedOperationException.</mark>


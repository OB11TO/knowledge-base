---
title: Класс Arrays
tags:
  - JavaSE
  - Array
related_topics: 
created: 2024-08-30 15:35
modified: 2025-01-13T17:37:56+03:00
difficulty: easy
questions: 
notes: 
links: 
---

## Класс Arrays, методы

java.util.Arrays

`asList()`: создает список из `массива.`
<mark class="hltr-red">При этом изменения в массиве будут видны в возвращаемом списке.</mark>

```java
String[] arr = {"apple", "banana", "orange"};
List<String> list = Arrays.asList(arr);
System.out.println(list); // [apple, banana, orange]
```

`binarySearch()`: осуществляет бинарный поиск значения в отсортированном массиве и возвращает индекс найденного значения.

```java
int[] arr = {1, 2, 3, 4, 5};
int index = Arrays.binarySearch(arr, 3);
System.out.println(index); // 2
```

`copyOf()`: создает новый массив указанной длины и копирует в него указанное количество элементов из исходного массива.

```java
int[] arr1 = {1, 2, 3};
int[] arr2 = Arrays.copyOf(arr1, 5);
System.out.println(Arrays.toString(arr2)); // [1, 2, 3, 0, 0]
```

`equals()`: сравнивает содержимое двух массивов.

```java
int[] arr1 = {1, 2, 3};
int[] arr2 = {1, 2, 3};
boolean equal = Arrays.equals(arr1, arr2);
System.out.println(equal); // true
```

`fill()`: заполняет массив указанным значением.

```java
int[] arr = new int[5];
Arrays.fill(arr, 1);
System.out.println(Arrays.toString(arr)); // [1, 1, 1, 1, 1]
```

`hashCode()`: возвращает хэш-код указанного массива.

```java
int[] arr = {1, 2, 3};
int hashCode = Arrays.hashCode(arr);
System.out.println(hashCode); // 30817
```

`parallelPrefix()`: осуществляет параллельное накопление операций в массиве, используя заданную функцию.

```java
int[] arr = {3, 1, 4, 2, 5};
Arrays.parallelSort(arr);
System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4, 5]
```

`parallelSetAll()`: осуществляет параллельное заполнение массива значениями, возвращаемыми заданной функцией.

```java
int[] arr = new int[5];
Arrays.parallelSetAll(arr, i -> i + 1);
System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4, 5]
```

`parallelSort()`: осуществляет параллельную сортировку массива в порядке возрастания.

```java
int[] arr = {3, 1, 4, 2, 5};
Arrays.parallelSort(arr);
System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4, 5]
```

`setAll()`: заполняет массив значениями, возвращаемыми заданной функцией.

```java
int[] arr = new int[5];
Arrays.setAll(arr, i -> i + 1);
System.out.println(Arrays.toString(arr)); // [1, 2, 3, 4, 5]
```

`spliterator()`: создает сплитератор для указанного массива.

```java
int[] arr = {1, 2, 3, 4, 5};
Spliterator.OfInt spliterator = Arrays.spliterator(arr);
spliterator.forEachRemaining(System.out::println); // 1 2 3 4 5
```

`stream()`: создает поток для указанного массива.

```java
int[] arr = {1, 2, 3, 4, 5};
IntStream stream = Arrays.stream(arr);
stream.forEach(System.out::println); // 1 2 3 4 5
```

`toString()`: возвращает строковое представление указанного массива.

```java
int[] arr = {1, 2, 3};
String str = Arrays.toString(arr);
System.out.println(str); // [1, 2, 3]
```

`mismatch` является статическим методом класса `Arrays` и используется для поиска первой позиции, на которой два указанных массива отличаются друг от друга.

```java
int[] arr1 = {1, 2, 3, 4, 5};
int[] arr2 = {1, 2, 3, 6, 7};
int index = Arrays.mismatch(arr1, arr2);
System.out.println(index); // 3
```
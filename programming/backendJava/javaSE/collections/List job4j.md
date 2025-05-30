---
title: List job4j
tags:
  - Collection
  - List
related_topics: 
created: 2025-02-24 12:00
modified: 2025-02-24T18:42:34+03:00
questions: 
notes: 
links: 
---


-----
##### Использовать итератор для доступа к элементам.

Для того чтобы получить экземпляр итератора в интерфейсе List<\E> определены 3 метода:

Iterator<\E> iterator() – метод возвращает объект Iterator, который содержит в себе все элементы исходной коллекции.
```java
Iterator<String> iterator = result.iterator();
while (iterator.hasNext()) {
    System.out.println("Текущий элемент: " + iterator.next());
}
```

ListIterator<\E> listIterator() – возвращает итератор списка для элементов в этом списке.
```java
ListIterator<String> iterator = result.listIterator();
while (iterator.hasNext()) {
    System.out.println("Текущий элемент: " + iterator.next());
}
```

ListIterator<\E> listIterator(int index) – возвращает итератор списка для элементов в этом списке, начиная с элемента индекс которого равен index.
```java
ListIterator<String> iterator = result.listIterator(2);
while (iterator.hasNext()) {
    System.out.println("Текущий элемент: " + iterator.next());
}
```



<mark class="hltr-red">Необходимо отметить, что ListIterator имеет несколько отличий от Iterator:</mark>
1. ListIterator может использоваться только со списками, т.е. реализациями интерфейса List<\E>, тогда как Iterator<\E> применим к любым наследникам и реализациям интерфейса Collection<E\>.
2. <mark class="hltr-green2">ListIterator позволяет перебирать список в обоих направлениях,</mark> Iterator<\E> только в прямом порядке.
3. <mark class="hltr-orange">ListIterator допускает модификацию списка, за счет его дополнительного метода add</mark>. Iterator<\E> такой возможности не имеет.

----
**ВАЖНО!** Метод <mark class="hltr-purple">remove(E e)</mark> реализован с помощью цикла for(), подразумевает под собой первоначальный поиск удаляемого элемента и только потом он удаляется. Соответственно, использование этого метода внутри цикла, который перебирает список, не рекомендуется, поскольку мы будем проходить по списку дважды.

------
**Отмеченные (*) методы** <mark class="hltr-green2">реализованы с помощью цикла for(), поэтому применять эти методы внутри циклов, которые перебирают список, нежелательно, поскольку так мы будем проходить по одному и тому же списку дважды.</mark>

![[Pasted image 20250224130416.png]]

-------

- ArrayList <mark class="hltr-green2">может хранить элементы абсолютно любых типов</mark>;
- ArrayList может<mark class="hltr-purple"> хранить значения null</mark>;
- ArrayList допускает <mark class="hltr-green2">хранение дубликатов</mark>.

Для создания экземпляра ArrayList в классе определено три конструктора:
1.1. ArrayList () - создается пустой список с <mark class="hltr-orange">начальной емкостью 10 элементов.</mark>
1.2. ArrayList(Collection<\? extends E> col) - создается список, в который помещается коллекция col.
1.3. ArrayList(int initialCapacity) - создается список с емкостью initialCapacity.

![[Pasted image 20250224182634.png]]

<mark class="hltr-red">Важно!</mark> Это замечание следует из предыдущего и из свойства списков хранить null. В контексте ArrayList, есть null элементы (это null элементы которые мы добавляем в список) и пустые ячейки контейнера, которые в конечном счете, тоже null. Не путайте null элементы и пустые ячейки контейнера! Пример, вы создали ArrayList начальной вместимостью 10, добавили в него 5 null элементов, несмотря на то, что внутри ArraList массив пуст (все ячейки null), у нас есть 5 null элементов, и в этой ситуации размер списка 5. Все это идет из того, что при вызове метода добавления происходит увеличение счетчика, отвечающего за количество элементов.

В классе `ArrayList`<mark class="hltr-green2"> при удалении элемента массив, используемый для хранения данных </mark>(`elementData`), <mark class="hltr-green2">не уменьшается в размере</mark>. Вместо этого, при удалении элемента происходит сдвиг последующих элементов влево, чтобы заполнить освободившееся место, и последний элемент устанавливается в `null`. Это освобождает ссылку на последний объект для сборщика мусора, но физический размер внутреннего массива остается прежним.

Если <mark class="hltr-yellow">вы хотите уменьшить фактический размер массива после удаления элементов, вы можете вызвать метод </mark>`trimToSize()`, который уменьшит емкость `ArrayList` до его текущего размера.


-------


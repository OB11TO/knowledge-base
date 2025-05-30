---
title: Iterator job4j
tags:
  - Collection
  - Job4j
related_topics: 
created: 2025-02-07 13:31
modified: 2025-02-12T14:51:14+03:00
difficulty: 
questions: 
notes: 
links: 
---


----

В основе этой <mark class="hltr-yellow">архитектурной идеи лежит разделение обязанностей между контейнером и механизмом обхода элементов.</mark> Интерфейс <mark class="hltr-red">Iterable</mark><mark class="hltr-green2"> служит контрактом для классов-контейнеров, позволяющим получить новый итератор каждый раз, когда вызывается метод iterator()</mark>. А вот <mark class="hltr-red">Iterator</mark> – э<mark class="hltr-purple">то объект, который хранит своё внутреннее состояние (т.е. позицию текущего обхода) и предназначен для одноразового прохода по элементам коллекции.</mark>

Если бы коллекция наследовалась напрямую от Iterator, то она сама бы являлась итератором, что привело бы к ряду проблем:

• **Невозможность повторного обхода.**  
Обычно ожидается, что вызов метода iterator() даст новый, «свежий» итератор, с которого можно начать обход с начала коллекции. Если бы коллекция была итератором (то есть возвращала бы себя), то повторный вызов метода iterator() просто продолжал бы использовать уже изменённое состояние – и обход уже завершён или находился бы в середине, что не соответствует контракту for‑each.

• **Проблемы с параллельной итерацией.**  
Если два потока или два независимых цикла пытаются итерировать одну и ту же коллекцию, то, возвращая один и тот же итератор (с состоянием, которое меняется), они будут мешать друг другу. Благодаря тому, что коллекция реализует Iterable, каждый вызов iterator() может возвращать новый независимый объект‑итератор.

• **Разделение ответственности.**  
Коллекция отвечает лишь за хранение элементов и за то, чтобы предоставить механизм (итератор) для их обхода. Логика обхода (перемещения по элементам, удаление текущего элемента и т.д.) инкапсулируется в объекте Iterator. Это повышает модульность и позволяет переиспользовать один и тот же механизм обхода для разных коллекций.

Как сказал один из ведущих экспертов (Jon Skeet),<mark class="hltr-orange"> если бы метод iterator() просто возвращал this (то есть коллекция была бы итератором), то повторный обход (например, два последовательных цикла for‑each) не смог бы корректно работать – второй цикл бы не увидел ни одного элемента, поскольку итератор уже «пройдён» ранее</mark>

----

#### **Валидация итератора.**
<mark class="hltr-red">Что будет, если в итераторе нет элементов и мы вызовем метод next? </mark>
<mark class="hltr-green2">В этом случае итератор должен сгенерировать исключение</mark> <mark class="hltr-pink">NoSuchElementException</mark>.
```java
@Test
void whenNextFromEmpty() {
    ArrayIterator iterator = new ArrayIterator(
            new int[] {}
    );
    assertThatThrownBy(iterator::next).isInstanceOf(NoSuchElementException.class);
}
```

-----

<mark class="hltr-green2">Шаблон итератор можно применить для любой структуры.</mark>
```java
int[][] input = {
        {1, 2},
        {3, 4}
};
```

-----

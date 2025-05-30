---
title: B-деревья B-trees
tags:
  - PostgreSql
related_topics: 
created: 2024-12-03 14:13
modified: 2025-04-21T10:47:05+03:00
questions: 
notes: 
links: 
---



![[Pasted image 20241203162718.png]]


### **Сбалансированное бинарное дерево**

**Сбалансированное бинарное дерево** — это бинарное дерево, в котором соблюдаются специальные правила, чтобы глубина (высота) дерева оставалась **минимальной** или **приблизительно одинаковой** для всех узлов.

#### **Свойства:**

- Разница между высотами левого и правого поддерева для любого узла **не превышает допустимого предела** (обычно 1 для строгих балансировок).
- Гарантируется эффективность операций:
    - **Поиск:** O(log⁡n)O(\log n)O(logn)
    - **Вставка:** O(log⁡n)O(\log n)O(logn)
    - **Удаление:** O(log⁡n)O(\log n)O(logn)

#### **Варианты сбалансированных деревьев:**

- **AVL-дерево** — строгий баланс: разница высот левого и правого поддеревьев любого узла не более 1.
- **Красно-чёрное дерево** — менее строгое, но быстрее восстанавливающее баланс после операций.
- **B-дерево** — используется в базах данных и файловых системах, балансируется с учётом хранения больших объёмов данных.

#### **Пример:**

```sql
       10
      /  \
     5    20
    / \   / \
   3   8 15  25

```


- Это дерево сбалансировано: высота левого и правого поддеревьев каждого узла различается не более чем на 1.

---

### **Сравнение бинарного дерева и сбалансированного бинарного дерева**

|**Характеристика**|**Бинарное дерево**|**Сбалансированное бинарное дерево**|
|---|---|---|
|**Правила размещения узлов**|Нет строгих ограничений|Гарантируется минимальная глубина дерева|
|**Эффективность операций**|Может быть O(n)O(n)O(n) в худшем случае|Гарантированно O(log⁡n)O(\log n)O(logn)|
|**Пример реализации**|Простое дерево, без балансировки|AVL, красно-чёрное, B-дерево|
|**Применение**|Используется для простых структур|Используется в системах, требующих быстрой работы|
|**Время восстановления баланса**|Не требуется|Выполняется при необходимости (после операций)|

---

### **Когда использовать каждое из них?**

- **Бинарное дерево**:
    
    - Подходит для простых задач с небольшим количеством данных, где эффективность не так важна.
    - Используется как базовая структура для изучения алгоритмов.
- **Сбалансированное бинарное дерево**:
    
    - Необходимо, когда важны гарантии производительности для операций поиска, вставки и удаления.
    - Используется в высоконагруженных системах, базах данных, компиляторах и файловых системах.



------------------

![[Pasted image 20241203163100.png]]

 ![[Pasted image 20241203163148.png]]

![[Pasted image 20241203163233.png]]
 ![[Pasted image 20241203163315.png]]

![[Pasted image 20241203163335.png]]

 ![[Pasted image 20241203163526.png]]

![[Pasted image 20241203163642.png]]


![[Pasted image 20241203163748.png]]

![[Pasted image 20241203163803.png]]

![[Pasted image 20241203163942.png]]

![[Pasted image 20241203164040.png]]
- <mark class="hltr-yellow">Массив плохо иметь, так как это не множество. Только когда ровно те записи, полное равенство</mark>. [1,2,3]  = [1,2,3]

### 1. **Массив как значение в индексе**

Если ты хранишь массив как часть значения в индексе (например, `int[]` или `varchar[]`), это возможно, но потенциально может вызывать проблемы:

#### Плюсы:

- Удобно, если массивы являются неотъемлемой частью данных и ты часто их ищешь как единое целое.
- Ускоряет поиск для конкретных массивов (например, найти запись, где массив равен `[1, 2, 3]`).

#### Минусы:

- **Низкая гранулярность**: Если требуется искать отдельные элементы массива (например, записи, где массив содержит число `5`), индекс становится неэффективным.
- **Больший размер индекса**: Массивы могут занять больше памяти, что усложняет поддержание структуры дерева.
- **Сложность сравнения**: Индексы сравниваются лексикографически (как строки или последовательности), что не всегда интуитивно.

![[Pasted image 20241203164146.png]]


![[Pasted image 20241203164351.png]]
![[Pasted image 20241203164401.png]]
![[Pasted image 20241203164432.png]]

- <mark class="hltr-red">Если два поля, то тогда нужно создать индекс для ДВУХ ПОЛЕЙ</mark>

![[Pasted image 20241203164455.png]]

---------

![[Pasted image 20241203164541.png]]
![[Pasted image 20241203164557.png]]
![[Pasted image 20241203164604.png]]
![[Pasted image 20241203164620.png]]


-------

![[Pasted image 20241203164652.png]]
![[Pasted image 20241203164709.png]]

![[Pasted image 20241203164739.png]]

---------

![[Pasted image 20241203164821.png]]

- <mark class="hltr-yellow">Применения дедуплекации</mark> 









----------------------------


![[Pasted image 20241204140122.png]]

![[Pasted image 20241204143105.png]]
![[Pasted image 20241204143312.png]]



-------------------------


![[Pasted image 20241204144207.png]]


-------------------

![[Pasted image 20241204145104.png]]


-------------


![[Pasted image 20241204150058.png]]


![[Pasted image 20241204150139.png]]

![[Pasted image 20241204150341.png]]


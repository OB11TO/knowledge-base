---
title: PriorityQueue
tags:
  - Collection
  - Queue
related_topics: 
created: 2024-09-09 17:57
modified: 2024-09-09T17:59:23+03:00
questions: 
notes: 
links: 
---
### class PriorityQueue\<E>

Класс `PriorityQueue<E>` представляет<mark class="hltr-yellow"> собой реализацию очереди с приоритетом</mark>(определяется на основе их "естественного порядка" или с использованием компаратора) в языке программирования Java. Вот некоторые плюсы и минусы этого класса:

```Java
public class Person {
    private String name;
    private double salary;}

PriorityQueue<Person> personPriorityQueue = new PriorityQueue<>();
personPriorityQueue.add(new Person("Vadim", 1000));

Exception in thread "main"
java.lang.ClassCastException:
class org.example.Person
 cannot be cast to class java.lang.Comparable
 (org.example.Person is in unnamed module of loader 'app';
 java.lang.Comparable is in module java.base of loader 'bootstrap')
```

```Java
public class Person implements Comparable<Person> {
    private String name;
    private double salary;
    
    // Конструкторы, геттеры и сеттеры
    
    @Override
    public int compareTo(Person otherPerson) {
        // Сравнение по полю salary
        if (this.salary < otherPerson.getSalary()) {
            return -1; // Текущий объект имеет меньшую заработную плату
        } else if (this.salary > otherPerson.getSalary()) {
            return 1; // Текущий объект имеет большую заработную плату
        } else {
            return 0; // Заработная плата равна
        }
    }
}
```

Плюсы:

1. Приоритетная очередь <mark class="hltr-orange">автоматически сортирует элементы на основе их приоритета</mark>. Это позволяет эффективно получать элементы с наивысшим приоритетом.
2. Класс `PriorityQueue<E>` <mark class="hltr-yellow">обеспечивает быструю вставку и извлечение элементов</mark>. <mark class="hltr-blue">Вставка</mark> элемента в приоритетную очередь имеет сложность <mark class="hltr-blue">O(log n)</mark>, а <mark class="hltr-green2">извлечение</mark> -<mark class="hltr-green2"> O(1).</mark>
3. Можно использовать `PriorityQueue<E>` для реализации алгоритмов, таких как <mark class="hltr-yellow">алгоритм Дейкстры</mark> или алгоритм А* (A-star), которым требуется обработка элементов в порядке их приоритета.
4. Класс `PriorityQueue<E>` предоставляет набор методов для работы с очередью, таких как добавление элемента (`add()`), удаление и возврат элемента с наивысшим приоритетом (`poll()`), проверка наличия элементов (`isEmpty()`), получение количества элементов (`size()`) и другие.

Минусы:

1. Приоритетная очередь <mark class="hltr-yellow">не является потокобезопасной.</mark> Если необходимо использовать `PriorityQueue<E>` в многопоточной среде, требуется синхронизация доступа к нему.
2. Класс `PriorityQueue<E>` <mark class="hltr-yellow">не допускает хранение дублирующихся элементов</mark>. Если необходимо хранить несколько элементов с одинаковым приоритетом, потребуется использовать дополнительные механизмы для разрешения коллизий.
3. Время доступа к произвольному элементу в приоритетной очереди не является эффективным. Если требуется частое обращение к элементам по их индексу, другие структуры данных, такие как список или массив, могут быть более подходящими.
4. <mark class="hltr-yellow">При изменении приоритета элемента</mark>, содержащегося в `PriorityQueue<E>`, <mark class="hltr-yellow">не происходит автоматическая пересортировка очереди</mark>. Это может потребовать дополнительных операций для поддержания корректного состояния приоритетной очереди.

---
title: Interfaces Comparable and Comparator
tags:
  - Collection
related_topics: 
created: 2024-09-09 16:21
modified: 2024-09-10T12:12:31+03:00
questions: 
notes: 
links: 
---
### Comparator\<T>

предназначен для сравнения <mark class="hltr-yellow">двух объектов типа T</mark>. Он <mark class="hltr-green2">определяет метод compare(T o1, T o2)</mark>, который <mark class="hltr-yellow">сравнивает два объекта и возвращает целое число, указывающее, какой из них должен быть отсортирован первым.</mark>

Метод compare должен возвращать отрицательное число, если o1 должен быть отсортирован перед o2, положительное число, если o1 должен быть отсортирован после o2, и ноль, если они равны.

Интерфейс <mark class="hltr-yellow">Comparator</mark>\<T> <mark class="hltr-green2">используется вместе с классами, которые не реализуют интерфейс</mark> <mark class="hltr-yellow">Comparable</mark>\<T>, <mark class="hltr-green2">чтобы определить порядок сортировки элементов.</mark> Классы, которые реализуют интерфейс Comparator\<T>, могут использоваться для сортировки массивов, списков и других коллекций.

### Методы

### Пример создания кастомного компаратора

```Java
package org.example;

public class Person {
    private String name;
    private double salary;

    public Person(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

@Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", salary=" + salary +
                '}';
    }
}
```

```Java
package org.example;

import java.util.Comparator;

public class PersonComparator implements Comparator<Person> {
    @Override
    public int compare(Person o1, Person o2) {
        if(o1.getSalary() > o2.getSalary()){
            return 1;
        }
        else if(o1.getSalary()< o2.getSalary()){
            return -1;
        }
        else return 0;
    }
    }
}
```

```Java
List<Person> persons = new ArrayList<>();
        persons.add(new Person("Vadim",1000));
        persons.add(new Person("Vova", 500));
        persons.add(new Person("Kostya", 300));
        persons.sort(new PersonComparator());
        persons.forEach(System.out::println);

ВЫВОД:
Person{name='Kostya', salary=300.0}
Person{name='Vova', salary=500.0}
Person{name='Vadim', salary=1000.0}
```

### Анонимный компаратор, использование лямбда выражения

<mark class="hltr-yellow">Анонимный класс:</mark>

```Java
List<Person> persons = new ArrayList<>();
        persons.add(new Person("Vadim",1000));
        persons.add(new Person("Vova", 500));
        persons.add(new Person("Kostya", 300));
        persons.sort(new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                if(o1.getSalary() > o2.getSalary()){
                    return 1;
                }
                else if(o1.getSalary()< o2.getSalary()){
                    return -1;
                }
                else return 0;
            }
        });
        }
    }
```

### Использование static метода класса Comparator

```Java
List<Person> persons = new ArrayList<>();
        persons.add(new Person("Vadim",1000));
        persons.add(new Person("Vova", 500));
        persons.add(new Person("Kostya", 300));
        persons.sort(Comparator.comparingDouble(Person::getSalary));
```

### Comparable\<T>

определяет единственный метод `compareTo`,<mark class="hltr-yellow"> который позволяет  
упорядочить объекты данного класса.</mark> Этот интерфейс позволяет сравнивать  объекты одного класса между собой, используя определенный порядок.  

- `int` [`compareTo`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html#compareTo-T-)`(`[`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html) `o)` метод принимает на вход объект типа `T` и возвращает целое число, которое указывает на отношение порядка между текущим объектом и переданным в метод объектом. Возвращаемое значение может быть отрицательным, нулевым или положительным:
- отрицательное число указывает на то, что текущий объект меньше переданного в метод;
- ноль указывает на то, что текущий объект равен переданному в метод;
- положительное число указывает на то, что текущий объект больше переданного в метод.

```Java
public class Person implements Comparable<Person> {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(Person other) {
        return name.compareTo(other.getName());
    }
}
```

```Java
List<Person> persons = new ArrayList<>();
persons.add(new Person("John"));
persons.add(new Person("Alice"));
persons.add(new Person("Bob"));
Collections.sort(persons);
System.out.println(persons); // [Alice, Bob, John]
```


### Отличия между Comparator and Comparable

В Java, интерфейсы `Comparator` и `Comparable` предназначены для сортировки объектов, но они имеют разные цели и используются в разных ситуациях. Вот основные различия:

## 1. Назначение

- **`Comparable`**: Этот интерфейс используется для естественной сортировки объектов. Он встроен в сам класс и определяет, как экземпляры этого класса должны быть сравнены. То есть объект сам знает, как его сортировать.
  
- **`Comparator`**: Этот интерфейс используется, когда нужно задать кастомный порядок сортировки, который отличается от естественного или когда класс не реализует `Comparable`. Он создается отдельно и передается для сортировки объектов извне.

## 2. Методы

- **`Comparable`**:
  - Этот интерфейс имеет один метод:
    ```java
    int compareTo(T obj);
    ```
    Метод сравнивает текущий объект с объектом `obj` и возвращает:
    - Отрицательное значение, если текущий объект меньше;
    - Ноль, если они равны;
    - Положительное значение, если текущий объект больше.

- **`Comparator`**:
  - Этот интерфейс имеет один основной метод:
    ```java
    int compare(T obj1, T obj2);
    ```
    Метод сравнивает два объекта `obj1` и `obj2` и возвращает:
    - Отрицательное значение, если `obj1` меньше;
    - Ноль, если они равны;
    - Положительное значение, если `obj1` больше.

## 3. Место определения сортировки

- **`Comparable`**: Определение сортировки находится внутри самого класса, реализующего интерфейс `Comparable`. Это естественная сортировка объектов.

  #### Пример:
  ```java
  public class Person implements Comparable<Person> {
      private String name;
      private int age;
  
      public Person(String name, int age) {
          this.name = name;
          this.age = age;
      }
  
      @Override
      public int compareTo(Person other) {
          return Integer.compare(this.age, other.age);  // Сортировка по возрасту
      }
  }
```

**`Comparator`**: Определение сортировки задается снаружи класса. Это удобно, когда нужно задать несколько способов сортировки или если класс не реализует `Comparable`

#### Пример:
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getters и другие методы
}

// Comparator для сортировки по имени
public class NameComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return p1.getName().compareTo(p2.getName());
    }
}

// Comparator для сортировки по возрасту
public class AgeComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return Integer.compare(p1.getAge(), p2.getAge());
    }
}

```

## 4. Пример использования

- **`Comparable`**: Используется, когда вы хотите, чтобы ваш класс поддерживал единственный способ сортировки (естественный порядок).
    
    #### Пример:

```java
List<Person> people = new ArrayList<>();
people.add(new Person("John", 30));
people.add(new Person("Alice", 25));

Collections.sort(people);  // Сортировка по возрасту, так как класс реализует Comparable

```

**`Comparator`**: Используется, когда нужно сортировать объекты различными способами или когда нельзя изменить исходный класс.

#### Пример:
```java
List<Person> people = new ArrayList<>();
people.add(new Person("John", 30));
people.add(new Person("Alice", 25));

// Сортировка по имени
Collections.sort(people, new NameComparator());

// Сортировка по возрасту
Collections.sort(people, new AgeComparator());

```

## 5. Гибкость

- **`Comparable`**: Предоставляет единственный способ сортировки, который встроен в сам класс. Если требуется другая логика сортировки, нужно изменить сам класс, что не всегда удобно.
    
- **`Comparator`**: Более гибкий, так как можно реализовать любое количество компараторов с разной логикой сортировки и применять их в зависимости от ситуации. Это особенно полезно, если вы хотите сортировать объекты по разным критериям.
    

## 6. Использование с Java 8+ (Лямбды)

С выходом Java 8 стало возможным использовать лямбда-выражения для реализации компараторов, что сделало работу с `Comparator` еще более удобной.

#### Пример:
```java
List<Person> people = new ArrayList<>();
people.add(new Person("John", 30));
people.add(new Person("Alice", 25));

// Сортировка по имени с помощью лямбда-выражения
people.sort((p1, p2) -> p1.getName().compareTo(p2.getName()));

// Сортировка по возрасту с помощью Comparator.comparing
people.sort(Comparator.comparing(Person::getAge));

```

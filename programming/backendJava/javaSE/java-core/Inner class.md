---
title: Inner class
tags:
  - JavaSE
  - InnerClass
related_topics: 
created: 2024-08-30 13:50
modified: 2025-03-26T12:34:14+03:00
difficulty: medium
questions: 
notes: 
links: 
---

### Внутренние классы/Inner( not static)

==Внутренние классы (или вложенные классы)== — классы, определенные внутри других классов. `Объекты внутренних классов вложены в объекты внешних классов и имеют доступ к их переменным и методам`. Вот основные моменты, которые следует запомнить:

- Внутренние классы объявляются внутри другого класса и ==могут обращаться к его переменным и методам.==
    ```java
    public class Car {
     int height = 160;
     ArrayList doors = new ArrayList();
    
     public Car  {
      doors.add(new Door());
     }
    
    class Door()  {
      public int getDoorHeight()  {
       return (int)(**height** * 0.80);
      }
     }
    }
    ```
    
- ==Объект типа внутреннего класса не может существовать отдельно от объекта внешнего класса.==
    Объект типа Door ==не может существовать отдельно от объекта типа Car – ведь он использует его переменные==. `Компилятор незаметно добавляет в конструктор и в класс Door ссылку на объект внешнего класса Car, чтобы методы внутреннего класса Door могли обращаться к переменным и вызвать методы внешнего класса Car`.
    
- Нельзя создать объект Door внутри статического метода в классе Car: негде взять ссылку на объект типа Car, который неявно передается в конструктор типа Door.
    
    ```java
    ПРАВИЛЬНО
    public class Car
    {
     public static Door createDoor()
     {
      **Car car = new Car();
      return car.new Door();**
     }
    
     public class Door
     {
      int width, height;
     }
    }
    ```
    
    ```java
    **НЕПРАВИЛЬНО**
    public class Car
    {
     public static Door createDoor()
     {
      return **new Door();**
     }
    
     public class Door
     {
      int width, height;
     }
    }
    ```
    
- Если внутренний класс объявлен как `public`, его объекты можно создавать вне внешнего класса, но объект внешнего класса при этом должен существовать.
    
    ```java
    Car car = new Car();
    Car.Door door = car.new Door();
    
    Car.Door door = new Car().new Door();
    ```
    
- ==Внутренние классы не могут содержать статические переменные и методы.==
    
    ```java
    
    **ПРАВИЛЬНО**
    public class Car
    {
     public int count;
     public int getCount()
     {
      return **count**;
     }
    
     public class Door
     {
      int width, height;
     }
    }
    ```
    
    ```java
    **НЕПРАВИЛЬНО**
    public class Car
    {
    
     public class Door
     {
      public **static** int count;
      int width, height;
    
      public **static** int getCount()
      {
       return count;
      }
     }
    }
    ```
    
- ==Внутренние классы могут иметь доступ к двум== `this` ссылкам: `this` относящемуся к внутреннему классу и `this` относящемуся к внешнему классу (при необходимости можно обратиться к внешнему `this` с помощью `ИмяВнешнегоКласса.this`).
    
    ```java
    Car.this
    
    Car.Door.this
    
    Car.Door.InnerClass2.InnerClass3.this
    ```
    
    ```java
    public class Car
    {
     int width, height;
    
     public class Door
     {
      int width, height;
    
      public void setHeight(int height)
      {
       this.height = height;
      }
    
      public int getHeight()
      {
       if (height!=0)
        return this.height;
       else
        return (int)(Car.this.height * 0.8);
      }
     }
    }
    ```


------

```java
Есть класс:

public class City {

    class Airport { }
 
}

Нужно создать класс 
class Miniport extends City.Airport{}

выберите правильный конструктор для данного класса:

public Miniport(City city) { city.super(); }

public Miniport(City city) { super(city); }

public Miniport(City city) { super().city; }
```


В Java, когда вы наследуете от внутреннего класса, <mark class="hltr-green2">конструктор производного класса должен явно вызвать конструктор базового класса, и при этом должен быть обеспечен экземпляр внешнего класса</mark> (в данном случае City), потому что базовый класс (Airport) является внутренним классом City.

Первый вариант: city.super(); 

Здесь используется <mark class="hltr-yellow">синтаксис вызова конструктора внутреннего класса через экземпляр внешнего класса</mark>. <mark class="hltr-purple">То есть, city.super() вызывает конструктор Airport </mark>(который является внутренним классом City) в контексте экземпляра city. Это правильный способ, потому что для создания экземпляра внутреннего класса Airport необходимо связать его с конкретным экземпляром City. Вызывая city.super(), мы говорим, что базовый класс Airport должен быть создан в контексте этого конкретного city. Поэтому такой конструктор корректен.

Второй вариант: super(city);

Это<mark class="hltr-yellow"> предполагает, что конструктор Airport принимает экземпляр City в качестве параметра. Но по определению класса Airport, он не имеет явного конструктора, принимающего City. Внутренние классы неявно получают ссылку на внешний класс, и их конструкторы по умолчанию ожидают эту неявную ссылку.</mark> То есть, <mark class="hltr-green2">конструктор Airport (как внутреннего класса) на самом деле имеет неявный параметр типа City.</mark> <mark class="hltr-red">Но при наследовании извне, в производном классе Miniport, мы должны явно передать этот параметр. Однако синтаксис super(city) будет работать только если конструктор Airport явно объявлен с параметром City, чего нет в данном случае. </mark>Поэтому такой вариант неверен, так как вызывает конструктор с несуществующим параметром.

Третий вариант: super().city;

Этот вариант вообще <mark class="hltr-yellow">синтаксически некорректен.</mark> super() вызывает конструктор базового класса (Airport) без аргументов, что невозможно, так как Airport как внутренний класс не имеет конструктора без аргументов (неявно требует экземпляр City). Даже если бы такой конструктор существовал, super().city попытается обратиться к полю city у Airport, которого нет. Поэтому этот вариант неправильный.
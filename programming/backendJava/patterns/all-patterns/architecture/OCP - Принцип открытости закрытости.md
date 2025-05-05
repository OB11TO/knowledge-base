---
title: OCP - Принцип открытости закрытости
tags:
  - Pattern
  - Architecture
related_topics: 
created: 2025-02-18 13:26
modified: 2025-02-27T10:54:11+03:00
questions: 
notes: 
links: 
---


------

Принцип <mark class="hltr-red">OCP – Open Closed Principle.</mark> Данный принцип г<mark class="hltr-green2">ласит, что программные сущности должны быть открыты к расширению, но закрыты к изменению.</mark>

Разберемся с тем,<mark class="hltr-yellow"> что значит «расширить», а что значит «изменить»</mark>. Взгляните на пример кода ниже:

```java
package ru.job4j.ood.ocp;

import java.util.function.BiFunction;

public class CalculatorExample {

    private static class SimpleCalculator {
        public int sum (int a, int b) {
            return a + b;
        }
    }

    private static class AbstractCalculator<T> {
        public T calculate(BiFunction<T, T, T> function, T first, T second) {
            return function.apply(first, second);
        }
    }
}
```

Допустим мне нужно будет добавить операцию вычитания в программу. <mark class="hltr-blue">Для первого класса мне нужно будет добавить еще метод</mark>, <mark class="hltr-red">т.е. его изменить</mark>. <mark class="hltr-blue">Для второго достаточно будет передать новую лямбду, т.</mark>е. <mark class="hltr-red">расширить программу, не изменяя ее.</mark>

<mark class="hltr-purple">Почему так важен факт изменения?</mark>

1) Изменение программы может привести к ее некорректной работе. Если программа маленькая, то продебажить ее не трудно, но если в ней много кода, то это будет сделать затруднительно, поэтому становится актуальным ее расширение, а не изменение.

2) Программы со временем меняются. Меняются требования. Выходят новые версии. Все это делается за счет возможности гибкого расширения программы.

-----

**Контекст расширения**

Говоря о расширении важно отметить контекст, в котором мы говорим о нем. В примере выше мы говорим об отдельно взятом внутреннем классе, а именно о внутреннем устройстве этих классов.

К примеру, кто-то может сказать: первый класс мы можем тоже расширить, например через наследование. Но тут уже другой контекст – контекст, в котором будет применяться сам класс. А это уже совсем другое. **Не путайте контекст.**

-------

##### Таким образом при написании расширяемых классов стоит обращать внимание:

- **на создаваемые объекты**.<mark class="hltr-yellow"> Помните классы должны зависеть от абстракций, а не от реализаций</mark> (принцип DIP). <mark class="hltr-red">Вспомните пример из прошлого задания. Подобный подход мешает наследованию</mark>

```java
package ru.job4j.ood.ocp;

import ru.job4j.ood.srp.SequenceGenerator;

import java.util.List;
import java.util.Random;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class SimpleSequenceGenerator implements SequenceGenerator<Integer> {

    @Override
    public List<Integer> generate(int size) {
        Random random = new Random();
        return IntStream.range(0, size)
                .map(i -> random.nextInt()).boxed()
                .collect(Collectors.toList());
    }
}
```

- **на поля**. <mark class="hltr-yellow">Поля также должны представлять собой тип абстракций</mark>. <mark class="hltr-red">Такой вариант тоже может привести к сложностям расширения, так как, чтобы расширить программу, придется наследоваться, но не всегда наследование возможно.</mark>

```java
package ru.job4j.ood.ocp;

import ru.job4j.ood.srp.SequenceGenerator;

import java.util.List;
import java.util.Random;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class SimpleSequenceGenerator implements SequenceGenerator<Integer> {

    private Random random;

    public SimpleSequenceGenerator(Random random) {
        this.random = random;
    }

    @Override
    public List<Integer> generate(int size) {
        return IntStream.range(0, size)
                .map(i -> random.nextInt()).boxed()
                .collect(Collectors.toList());
    }
}
```

- **на параметры и возвращаемые типы методов**.<mark class="hltr-yellow"> Они тоже должны быть абстракциями. Как в примере выше</mark>.

------

#### **Механизмы расширения**

<mark class="hltr-green2">Расширение, как правило, достигается созданием новых сущностей.</mark> Тут мы ярко можем увидеть полиморфизм. Таким образом,<mark class="hltr-green2"> расширение достигается за счет полиморфизма</mark>. <mark class="hltr-purple">А за счет чего достигается полиморфизм?
</mark>

_Наследование_  <mark class="hltr-red">!!!!!!!!!!</mark>
Пусть изначально был написан такой код
```java
package ru.job4j.ood.ocp;

import java.util.List;

public class CarsInheritance {

    private static class Car {
        public String sound() {
            return "beep-beep";
        }
    }

    public static void main(String[] args) {
        List<Car> cars = List.of(new Car(), new Car());
        cars.forEach(Car::sound);
    }
}
```

Теперь поступило требование, что некоторые машины не издают звука<mark class="hltr-yellow">. Как этот код можно расширить, реализуя требование? Можно расширить за счет наследования, создав новую сущность и добавив ее в список.</mark>

```java
package ru.job4j.ood.ocp;

import java.util.List;

public class CarsInheritance {

    private static class Car {
        public String sound() {
            return "beep-beep";
        }
    }

    private static class NoSoundCar extends Car {
        @Override
        public String sound() {
            return "";
        }
    }

    public static void main(String[] args) {
        List<Car> cars = List.of(new Car(), new Car(), new NoSoundCar());
        cars.forEach(Car::sound);
    }
}
```


_Интерфейсы и их реализация_  <mark class="hltr-red">!!!!!!!!!!</mark>

Пусть изначально был написан такой код
```java
package ru.job4j.ood.ocp;

import java.util.List;

public class FiguresImplemetation {

    private static class Rectangle {
        public String draw() {
            return "...";
        }
    }

    public static void main(String[] args) {
        List<Rectangle> rectangles = List.of(new Rectangle());
        rectangles.forEach(Rectangle::draw);
    }
}
```

<mark class="hltr-green2">Теперь поступило требование рисовать круги. Можно ли здесь использовать наследование? Нет. Т.к. при наследовании, наследуется состояние объекта предка</mark>. Круг не может иметь те же характеристики, что и прямоугольник, потому что круг «не является» прямоугольником (отношение «is A» наследование), но они оба могут быть отрисованы. <mark class="hltr-red">Таким образом исходный код изначально спроектирован неверно. Нужно было сделать так:</mark>

```java
package ru.job4j.ood.ocp;

import java.util.List;

public class FiguresImplemetation {

    private interface Drawable {
        String draw();
    }

    private static class Rectangle implements Drawable {
        @Override
        public String draw() {
            return "...";
        }
    }

    public static void main(String[] args) {
        List<Drawable> rectangles = List.of(new Rectangle());
        rectangles.forEach(Drawable::draw);
    }
}
```

**Вывод**,<mark class="hltr-green2"> наследование можно использовать только при устойчивой иерархии классов, всегда подставляйте «is A» между сущностями, чтобы проверить можно ли наследоваться.</mark> Если не уверены, то лучше использовать интерфейсы, потому что они дают гибкость. Также помните, что при наследовании наследуется состояние объектов, что не всегда удобно, нужно и допустимо.

<mark class="hltr-orange">!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!</mark>
В языке Java классы верхнего уровня не могут быть объявлены как `static`. Это связано с тем, что модификатор `static` применяется для членов класса, указывая на их принадлежность самому классу, а не его экземплярам. Поскольку класс верхнего уровня не является членом другого класса, применение к нему модификатора `static` не имеет смысла и не допускается синтаксисом Java.
<mark class="hltr-orange">!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!</mark>

-----

<mark class="hltr-red">Вот три примера нарушения принципа OCP (Open/Closed Principle) на Java, где для добавления новой функциональности приходится модифицировать уже существующий код</mark> 🔥:


### 1️⃣ Пример с вычислением площади фигур 📐

**Описание:**  
Если у вас есть класс для вычисления площади разных фигур и вы реализуете логику через `switch` или `if-else`, то при добавлении новой фигуры вам придётся модифицировать существующий метод, что нарушает принцип закрытости для модификаций 🚫

```java
public enum ShapeType {
    CIRCLE, SQUARE
}

public class Shape {
    private ShapeType type;
    private double dimension;

    public Shape(ShapeType type, double dimension) {
        this.type = type;
        this.dimension = dimension;
    }

    public ShapeType getType() {
        return type;
    }

    public double getDimension() {
        return dimension;
    }
}

public class AreaCalculator {
    public double calculateArea(Shape shape) {
        switch (shape.getType()) {
            case CIRCLE:
                return Math.PI * Math.pow(shape.getDimension(), 2);
            case SQUARE:
                return Math.pow(shape.getDimension(), 2);
            default:
                throw new IllegalArgumentException("Unsupported shape type");
        }
    }
}

```

_Нарушение:_ Добавление новой фигуры (например, прямоугольника) потребует изменений в методе `calculateArea()` ✂️.

-----

### 2️⃣ Пример с вычислением скидки для клиентов 💸

**Описание:**  
Если логика расчёта скидки для разных типов клиентов реализована в одном классе с использованием условных операторов, то для каждого нового типа клиента потребуется изменение этого класса 🔄.

```java
public class Customer {
    private String type;
    private double totalAmount;

    public Customer(String type, double totalAmount) {
        this.type = type;
        this.totalAmount = totalAmount;
    }

    public String getType() {
        return type;
    }

    public double getTotalAmount() {
        return totalAmount;
    }
}

public class DiscountCalculator {
    public double calculateDiscount(Customer customer) {
        if ("REGULAR".equals(customer.getType())) {
            return 0;
        } else if ("VIP".equals(customer.getType())) {
            return customer.getTotalAmount() * 0.1;
        }
        // При добавлении нового типа клиента придется модифицировать этот метод!
        return 0;
    }
}

```

_Нарушение:_ Добавление нового типа скидок ведёт к изменению метода `calculateDiscount()` вместо расширения функциональности 👎.

---
### 3️⃣ Пример с логированием сообщений 📝

**Описание:**  
Класс, который логирует сообщения в зависимости от типа логирования, может использовать условные операторы. При добавлении нового способа логирования придется править метод логирования, что нарушает принцип OCP 🔧.

```java
public class Logger {
    public void log(String message, String logType) {
        if ("FILE".equalsIgnoreCase(logType)) {
            // Запись в файл
            System.out.println("Log in file: " + message);
        } else if ("CONSOLE".equalsIgnoreCase(logType)) {
            // Вывод в консоль
            System.out.println("Log in console: " + message);
        }
        // Добавление нового типа логирования потребует изменения этого метода
    }
}

```

_Нарушение:_ Каждый раз при добавлении нового логгера (например, для базы данных) метод `log()` придется менять, а не расширять через наследование или интерфейсы 🌱.

-----

## 1️⃣ Вычисление площади фигур 📐

**Решение:**  
Вводим интерфейс `Shape` с методом `area()`. Каждая фигура реализует свой способ вычисления площади, что позволяет добавлять новые фигуры без модификации кода калькулятора 🚀:

```java
// Интерфейс для фигур
public interface Shape {
    double area();
}

// Класс для круга
public class Circle implements Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

// Класс для квадрата
public class Square implements Shape {
    private double side;
    
    public Square(double side) {
        this.side = side;
    }
    
    @Override
    public double area() {
        return side * side;
    }
}

// Калькулятор площади, который использует полиморфизм
public class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.area();
    }
}

```

_Плюс:_ Добавить новую фигуру (например, прямоугольник) можно, создав новый класс, реализующий интерфейс `Shape`, без изменений в `AreaCalculator` 🎉.

---

## 2️⃣ Вычисление скидки для клиентов 💸

**Решение:**  
Применяем шаблон «Стратегия». Создаем интерфейс `DiscountStrategy` и отдельные классы для каждого типа скидок. Класс `DiscountCalculator` выбирает стратегию по типу клиента, не меняя свой основной код 🌱:

```java
// Интерфейс стратегии расчёта скидки
public interface DiscountStrategy {
    double calculateDiscount(Customer customer);
}

// Стратегия для обычного клиента
public class RegularDiscountStrategy implements DiscountStrategy {
    @Override
    public double calculateDiscount(Customer customer) {
        return 0;
    }
}

// Стратегия для VIP клиента
public class VipDiscountStrategy implements DiscountStrategy {
    @Override
    public double calculateDiscount(Customer customer) {
        return customer.getTotalAmount() * 0.1;
    }
}

// Класс клиента
public class Customer {
    private String type;
    private double totalAmount;
    
    public Customer(String type, double totalAmount) {
        this.type = type;
        this.totalAmount = totalAmount;
    }
    
    public String getType() {
        return type;
    }
    
    public double getTotalAmount() {
        return totalAmount;
    }
}

// Калькулятор скидок, использующий стратегии
import java.util.Map;
import java.util.HashMap;

public class DiscountCalculator {
    private Map<String, DiscountStrategy> strategies = new HashMap<>();
    
    public DiscountCalculator() {
        strategies.put("REGULAR", new RegularDiscountStrategy());
        strategies.put("VIP", new VipDiscountStrategy());
    }
    
    public double calculateDiscount(Customer customer) {
        DiscountStrategy strategy = strategies.get(customer.getType());
        if (strategy == null) {
            return 0; // или можно бросить исключение, если стратегия не найдена
        }
        return strategy.calculateDiscount(customer);
    }
}

```
_Плюс:_ При необходимости добавления нового типа клиента — достаточно создать новую стратегию и зарегистрировать её в мапе, не трогая логику метода `calculateDiscount()` 🔥.

---

## 3️⃣ Логирование сообщений 📝

**Решение:**  
Используем подход с интерфейсом `LogStrategy`, который позволяет создавать различные реализации логирования (например, в файл, консоль, базу данных и т.д.). Класс `Logger` принимает стратегию логирования через конструктор, что позволяет легко подменять её без изменения кода самого логгера 🚀:

```java
// Интерфейс стратегии логирования
public interface LogStrategy {
    void log(String message);
}

// Реализация логирования в файл
public class FileLogStrategy implements LogStrategy {
    @Override
    public void log(String message) {
        // Пример: запись в файл (упрощенно)
        System.out.println("File log: " + message);
    }
}

// Реализация логирования в консоль
public class ConsoleLogStrategy implements LogStrategy {
    @Override
    public void log(String message) {
        System.out.println("Console log: " + message);
    }
}

// Класс логгера, использующий стратегию
public class Logger {
    private LogStrategy logStrategy;
    
    public Logger(LogStrategy logStrategy) {
        this.logStrategy = logStrategy;
    }
    
    public void log(String message) {
        logStrategy.log(message);
    }
}

```
_Плюс:_ Для добавления нового способа логирования, создайте новую реализацию `LogStrategy` и передайте её в `Logger` — никаких изменений в существующем коде не потребуется 🎉.

------

![[Pasted image 20250224203556.png]]
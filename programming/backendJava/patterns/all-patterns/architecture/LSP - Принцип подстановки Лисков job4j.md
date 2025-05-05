---
title: LSP - Принцип подстановки Лисков job4j
tags:
  - Pattern
  - Architecture
related_topics: 
created: 2025-02-24 19:51
modified: 2025-02-25T16:20:27+03:00
questions: 
notes: 
links: 
---


------
[[LSP - Принцип подстановки Лисков более детальнее]]





------

В этом уроке поговорим про<mark class="hltr-red"> LSP (Liskov Substitution Principle)</mark>. Данный принцип<mark class="hltr-green2"> гласит, что если в коде используется  сущность X, то при постановке его наследников или других реализаций Y код будет работать</mark>. <mark class="hltr-yellow">Фактически этот принцип гарантирует, что не нарушится принцип OCP относительно взаимосвязи между классами в иерархии.</mark>

<mark class="hltr-red">Как проверить что ваш код не нарушает этот принцип? </mark>Легко, <mark class="hltr-green2">представляете что нужно будет использовать в коде какие-то специфические  
реализации и пытаетесь мысленно их подставить</mark>. Если нет проблем с подстановкой, то все отлично. В целом не должно быть завязки на конкретной реализации.

**Контракты LSP**
_Контракт_ - это некое правило, соблюдение которого гарантирует, что принцип не будет нарушен.  
<mark class="hltr-purple">Применительно LSP выделяют следующие контракты:</mark>

<mark class="hltr-orange">_1. Предусловия (Preconditions) не могут быть усилены в подклассе_</mark>
<mark class="hltr-red">_Предусловие_</mark> - это<mark class="hltr-green2"> условие, которое проверяет корректность аргументов конструктора или метода.</mark>

Допустим у нас есть класс автотранспорта. Пусть теперь есть наследник - автобус. Предположим ему нужно больше топлива, чтобы сдвинуться с места
```java
package ru.job4j.ood.lsp;
 
class AutoTransport {

    protected float fuel;

    public AutoTransport(float fuel) {
        this.fuel = fuel;
    }

    public void move(float km) {
        if (km < 0) {
            throw new IllegalArgumentException("Invalid distance!");
        }
        if (fuel < 0) { /* <= предусловие */
            throw new IllegalArgumentException("Need more fuel!");
        }
        /* other logic */
    }

}

class Bus extends AutoTransport {

    public Bus(float fuel) {
        super(fuel);
    }

    public void move(float km) {
        if (km < 0) {
            throw new IllegalArgumentException("Invalid distance!");
        }
        if (fuel < 5) { /* условие усилено */
            throw new IllegalArgumentException("Need more fuel!");
        }
        /* other logic */
    }

}

public class FirstRule {
    public static void main(String[] args) {
        AutoTransport bus = new Bus(3);
        bus.move(2);
    }
}
```

От AutoTransport мы ожидаем, что машина сдвинется, но нет. Автобус не сдвигается, т.к. в нем усилено предусловие. Ожидаем мы одно поведение, а получаем другое.  
Чтобы сдвинуть автобус нам придется дописать доп. условие, чтобы проверить является ли транспорт автобусом. Далее уже скормить ему больше топлива.


----

<mark class="hltr-orange">_2. Постусловия (Postconditions) не могут быть ослаблены в подклассе._</mark>

<mark class="hltr-red">_Постусловие_</mark> - это <mark class="hltr-green2">условие, налагаемое на возвращаемое значение метода.</mark>

Рассмотрим такой пример. Пусть есть бухгалтерия, которая считает по табелю сколько работник отработал  
и если он отработал норму, то высчитывает для него зарплату. Далее мы наследуемся допустим для бухгалтерии магазина. В нем мы забываем про условие когда добавляем специфическое поведение  
и когда запускаем пример, то получаем, что недобросовестный работник получает зарплату.

```java
package ru.job4j.ood.lsp;

import java.time.LocalDate;
import java.time.Month;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.Map;

class WorkDays implements Iterable<Integer> {

    private Map<LocalDate, Integer> workDays = new LinkedHashMap<>();

    public void add(LocalDate date, int hours) {
        workDays.put(date, hours);
    }

    @Override
    public Iterator<Integer> iterator() {
        return workDays.values().iterator();
    }
}

class CountingRoom {

    protected int normHours;

    protected int payPerHour;

    public CountingRoom(int normHours, int payPerHour) {
        this.normHours = normHours;
        this.payPerHour = payPerHour;
    }

    public int pay(WorkDays workDays) {
        int factHours = 0;
        for (Integer hoursPerDay : workDays) {
            factHours += hoursPerDay;
        }
        if (factHours < normHours) {
            throw new IllegalArgumentException("Worker didn't work enough!");
        }
        return factHours * payPerHour;
    }

}

class ShopCountingRoom extends CountingRoom {

    public ShopCountingRoom(int normHours, int payPerHour) {
        super(normHours, payPerHour);
    }

    @Override
    public int pay(WorkDays workDays) {
        int factHours = 0;
        for (Integer hoursPerDay : workDays) {
            factHours += hoursPerDay;
        }
        return factHours * payPerHour;
    }
}

public class SecondRule {
    public static void main(String[] args) {

        WorkDays workDays = new WorkDays();
        workDays.add(LocalDate.of(2020, Month.DECEMBER, 1), 8);
        workDays.add(LocalDate.of(2020, Month.DECEMBER, 2), 6);
        workDays.add(LocalDate.of(2020, Month.DECEMBER, 3), 7);

        CountingRoom countingRoom = new ShopCountingRoom(3 * 8, 500);
        System.out.println(countingRoom.pay(workDays));
    }
}
```

**Правила 1-2 не распространяются на приватные поля, т.е. когда вы проверяете специфичные только для объекта поля, то вы не нарушаете эти правила.** **Принцип Лисков контролирует отношения между классами при наследовании.**

------

<mark class="hltr-orange">_3. Инварианты (Invariants) — все условия базового класса также должны быть сохранены и в подклассе_</mark>

<mark class="hltr-red">_Инвариант_</mark> - это<mark class="hltr-green2"> условие, которое постоянно на протяжении существования объекта</mark>.

Например, есть номер телефона. Есть абонент. От него наследуется абонент какого-то оператора. Но при переопределении сеттера, забыли сделать проверку. Поэтому код в машине запускается успешно.  
Ошибка остается. Нарушено состояние объекта потомка, потому что в нем не соблюдено условие предка.

```java
package ru.job4j.ood.lsp;


class PhoneNumber {

    private int countryCode;

    private int cityCode;

    private int number;

    public PhoneNumber(int countryCode, int cityCode, int number) {
        this.countryCode = countryCode;
        this.cityCode = cityCode;
        this.number = number;
    }

    public int getCountryCode() {
        return countryCode;
    }

    public int getCityCode() {
        return cityCode;
    }

    public int getNumber() {
        return number;
    }
}

/* абонент */
class Subscriber {

    protected PhoneNumber phoneNumber;

    public Subscriber(PhoneNumber phoneNumber) {
        validate(phoneNumber);
        this.phoneNumber = phoneNumber;
    }

    protected void validate(PhoneNumber phoneNumber) {
        if (phoneNumber.getCountryCode() < 1 || phoneNumber.getCountryCode() > 999) {
            throw new IllegalArgumentException("Invalid country code!");
        }
        if (phoneNumber.getCityCode() < 1 || phoneNumber.getCityCode() > 999) {
            throw new IllegalArgumentException("Invalid city code!");
        }
        if (phoneNumber.getNumber() < 1 || phoneNumber.getNumber() > 999_999_999) {
            throw new IllegalArgumentException("Invalid number!");
        }
    }

    public PhoneNumber getPhoneNumber() {
        return phoneNumber;
    }

    public void setPhoneNumber(PhoneNumber phoneNumber) {
        validate(phoneNumber);
        this.phoneNumber = phoneNumber;
    }
}

class SomeOperatorSubscriber extends Subscriber {

    public SomeOperatorSubscriber(PhoneNumber phoneNumber) {
        super(phoneNumber);
    }

    @Override
    public void setPhoneNumber(PhoneNumber phoneNumber) {
        /* some specific logic; */
        /* Забыли сделать проверку. Возможно не валидное состояние */
        this.phoneNumber = phoneNumber;
    }
}

public class ThirdRule {
    public static void main(String[] args) {
        Subscriber subscriber = new SomeOperatorSubscriber(
                new PhoneNumber(+1, 111, 111_111_111)
        );
        subscriber.setPhoneNumber(
                new PhoneNumber(-1, 111, 111_111_111)
        );
    }
}
```

------------

**Признаки нарушения принципа:**

<mark class="hltr-red">_1. Использование методов getClass(), instance of_</mark>

Допустим вам нужно написать программу которая рисует фигуры. У каждой фигуры есть свое состояние 
Написание кода вида

```java
        if (figure.getClass() == Line.class) {
            canvas.drawLine(
                    ((Line) figure).getX1(), ((Line) figure).getY1(),
                    ((Line) figure).getX1(), ((Line) figure).getY1()
            );
        } else if (figure.getClass() == Rectangle.class) {
             ...
        } else if (...) {

        }
        ...
```

Приведет к нарушению принципа OCP, мы не можем расширить код, нам нужно его дописать чтобы он работал с новой фигурой, а  
нарушение принципа OCP было по причине нарушения принципа LSP.

Аналогично было бы нарушением использование instance of

<mark class="hltr-red">_2. Написание кода с наличием слова type_</mark>

Часто код где есть методы setType(), getType() нарушает принцип LSP. Исключением здесь является использование рефлекции, которая само  
собой предполагает извлечение и модификацию метаинформации о классах.

Обычно совместно с этим идет использование enum. Например для примера выше могло бы быть создано перечисление

```java
enum FigureType {
    LINE, RECTANGLE, ...
}
```

и тот же самый код уже будет выглядеть
```java
        if (figure.getType() == FigureType.LINE) {
            canvas.drawLine(
                    ((Line) figure).getX1(), ((Line) figure).getY1(),
                    ((Line) figure).getX1(), ((Line) figure).getY1()
            );
        } else if (FigureType.RECTANGLE) {
            ...
        } else if (...) {

        }
        ...
```

Это не означает, что нужно избегать использования enum, это означает, что нужно тщательно подумать прежде чем ограничивать  
реализации. В enum должны быть константы, которые в будущем не будут изменяться и не будут пополняться.

<mark class="hltr-red">_3. Обращение к свойствам ограничивающим определенное множество объектов._</mark>

К примеру, у каждой фигуры замкнутой фигуры есть площадь. Вы решили по разному рисовать фигуры с разной площадью

```java
        if (figure.isClosed()) {
            double s = figure.square();
            if (100 < s && s < 500) {
                // draw red
            } else if (...) {
               ...
            }
        }
```

Такой код может и не нарушить LSP, если учесть все варианты. Но в таких случаях высока вероятность допустить ошибку. Будь бдительны. Подумайте не понадобятся ли в дальнейшем посылать в метод объекты с другими свойствами.

**Вывод:**

1. Если при подстановке нарушается поведение объекта, то это есть нарушение принципа Лисков.  
2. Если в коде присутствует адаптация под реализации, то скорее всего код нарушает принцип Лисков. Это наблюдение исходит из первого, потому что под определенное поведение нам нужна определенная проверка.




------------------------


## Проблемы и их решения 

## 1️⃣ Случай: Ужесточение предусловий ⏮️🚫

**Проблема:**  
Предположим, у нас есть базовый класс, где метод принимает достаточно широкий диапазон входных данных, а в подклассе мы вводим более строгие требования.

```java
// Базовый класс
class Vehicle {
    // Метод стартует двигатель, если уровень топлива > 0
    public void startEngine(double fuelLevel) {
        if (fuelLevel <= 0) {
            throw new IllegalArgumentException("Нет топлива!");
        }
        System.out.println("Двигатель запущен 🚗");
    }
}

// Подкласс, где мы ужесточили предусловия
class ElectricVehicle extends Vehicle {
    @Override
    public void startEngine(double fuelLevel) {
        // Здесь вместо топлива требуем заряд батареи > 20%
        if (fuelLevel <= 20) {
            throw new IllegalArgumentException("Низкий заряд батареи ⚡");
        }
        System.out.println("Электродвигатель запущен 🚙");
    }
}

```

**Почему LSP нарушен?**  
Код, который использует объект типа `Vehicle`, может вызвать метод `startEngine` с уровнем, например, 10, ожидая, что объект корректно запустится. Но если подставить `ElectricVehicle`, то условие станет жестче – и метод выбросит исключение!

**Решение:**  
Подкласс **не должен** ужесточать предусловия базового класса. Решить проблему можно, либо не переопределять метод, либо ослабить условие до уровня базового класса:

--------------
## 2️⃣ Случай: Нарушение в классе «Квадрат» и «Прямоугольник» 📏🔄

**Проблема:**  
Класс `Rectangle` позволяет независимо задавать ширину и высоту. При наследовании класс `Square` (Квадрат) вынужден переопределять методы так, что изменение одной стороны влияет на другую.  
Пример:

```java
// Базовый класс
class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int width) {
        this.width = width;
    }
    
    public void setHeight(int height) {
        this.height = height;
    }
    
    public int getArea() {
        return width * height;
    }
}

// Подкласс «Квадрат»
class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width; // Принудительно делаем стороны равными
    }
    
    @Override
    public void setHeight(int height) {
        this.height = height;
        this.width = height; // Принудительно делаем стороны равными
    }
}

```

**Почему LSP нарушен?**  
Код, работающий с объектом `Rectangle`, может ожидать, что методы `setWidth` и `setHeight` работают независимо. При подстановке объекта `Square` такое поведение изменяется – стороны всегда равны, что может привести к неожиданным результатам.

**Решение:**  
Избегать наследования, которое нарушает ожидания клиентов. Можно:

- Не наследовать `Square` от `Rectangle`, а сделать их отдельными классами с общим интерфейсом, либо
- Применить композицию, чтобы избежать нежелательных побочных эффектов.

------

## 3️⃣ Случай: Ослабление постусловий ⏭️⚠️

**Проблема:**  
Базовый класс гарантирует определённое поведение метода (например, всегда возвращает не-null значение), а подкласс изменяет логику и может возвращать `null`.  
Пример:

```java
// Базовый класс
class Cache {
    // Гарантирует, что если элемент найден, он не равен null
    public Object get(String key) {
        Object value = findValue(key);
        if (value == null) {
            throw new IllegalStateException("Значение не найдено!");
        }
        return value;
    }
    
    protected Object findValue(String key) {
        // Поиск значения...
        return new Object();
    }
}

// Подкласс, который ослабил постусловие
class NullableCache extends Cache {
    @Override
    public Object get(String key) {
        // Возвращаем null, если элемент не найден, не выбрасывая исключение
        return findValue(key);
    }
}

```

**Почему LSP нарушен?**  
Клиенты, использующие `Cache`, ожидают, что метод `get` никогда не вернёт `null` (иначе возникает ошибка). При подстановке объекта `NullableCache` контракт нарушается – и могут возникнуть NPE (NullPointerException).

**Решение:**  
Подкласс должен соблюдать или усиливать постусловия базового класса. Если базовый метод гарантирует не-null, подкласс обязан это соблюдать.  
Можно либо не изменять контракт, либо добавить дополнительные гарантии, но не ослаблять:

```java
class NullableCacheFixed extends Cache {
    @Override
    public Object get(String key) {
        Object value = findValue(key);
        if (value == null) {
            // Альтернативная обработка, например, возвращаем значение по умолчанию
            return new Object(); // или выбрасываем исключение, как в базовом классе
        }
        return value;
    }
}

```

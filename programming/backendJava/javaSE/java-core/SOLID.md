---
title: SOLID
tags:
  - Pattern
  - Architecture
related_topics: 
created: 2024-08-31 13:37
modified: 2025-03-17T12:42:02+03:00
difficulty: 
questions: 
notes: 
links: 
---



-------------
[[SRP - Принцип единственной ответственности job4j]]
[[OCP - Принцип открытости закрытости]]
[[LSP - Принцип подстановки Лисков job4j]]
ISP - Принцип разделения интерфейсов job4j
[[DIP -Принцип инверсии зависимостей job4j]]



----------------------


# SOLID
### Singl Responsobility
Рассмотрим первый принцип - **принцип единственной ответственности** на примере.

Допустим у нас есть класс RentCarService и в нем есть несколько методов: найти машину по номеру, забронировать машину, распечатать заказ, получить информацию о машине, отправить сообщение.
![[Pasted image 20240909141330.png]]
<mark class="hltr-yellow">У данного класса есть несколько зон ответственности, что является нарушением первого принципа</mark>. Возьмем метод получения информации об машине. Теперь у нас есть только три типа машин sedan, pickup и van, но если Заказчик захочет добавить еще несколько типов, тогда придется изменять и дописывать данный метод.

Или возьмем метод отправки сообщения. Если кроме отправки сообщения по электронной почте необходимо будет добавить отправку смс, то также необходимо будет изменять данный метод.

<mark class="hltr-green2">Одним словом, данный класс нарушает принцип единой ответственности, так как отвечает за разные действия.</mark>

Необходимо разделить данный класс RentCarService на несколько, и тем самым, следуя принципу единой ответственности, предоставить каждому классу отвечать только за одну зону или действие, так в дальнейшем его будет проще дополнять и модифицировать.

![[Pasted image 20240909141356.png]]
![[Pasted image 20240909141420.png]]
![[Pasted image 20240909141431.png]]


### Принцип открытости/закрытости (OCP)

Этот принцип гласит, что <mark class="hltr-yellow">классы должны быть открыты для расширения, но закрыты для изменения</mark>. Это означает, что <mark class="hltr-green2">мы должны быть в состоянии добавлять новое поведение в существующие классы без изменения их исходного кода</mark>.

**`Пример:`**

Предположим, у нас есть класс для вычисления зарплаты сотрудника:

```java
class Employee {
    private double salary;

    public Employee(double salary) {
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }
}

```

Теперь добавим класс для вычисления бонусов:

```java

interface Bonus {
    double calculate(Employee employee);
}

class FixedBonus implements Bonus {
    public double calculate(Employee employee) {
        return employee.getSalary() * 0.1; // 10% фиксированный бонус
    }
}

class PerformanceBonus implements Bonus {
    public double calculate(Employee employee) {
        return employee.getSalary() * 0.2; // 20% бонус за производительность
    }
}

```

Мы <mark class="hltr-yellow">можем</mark><mark class="hltr-yellow"> добавить новые классы бонусов, не изменяя существующие классы</mark> `Employee` или `Bonus`.

### Принцип подстановки Лисков (LSP)

Этот принцип говорит о том, что п<mark class="hltr-yellow">одклассы должны дополнять, а не изменять поведение базовых классов, то есть объекты подклассов должны заменять объекты базовых классов без изменения корректности программы.</mark>

**`Пример:`**

Предположим, у нас есть класс `Bird` и его подкласс `Penguin`:

```java

class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly");
    }
}

```

Этот пример нарушает LSP, потому что `Penguin` <mark class="hltr-green2">не может быть подставлен в код, ожидающий объект типа</mark> `Bird`, <mark class="hltr-green2">без изменения поведения программы.</mark>

Лучший подход будет <mark class="hltr-yellow">использовать интерфейсы или абстрактные классы:</mark>

```java

interface Bird {
    void move();
}

class Sparrow implements Bird {
    @Override
    public void move() {
        System.out.println("Flying");
    }
}

class Penguin implements Bird {
    @Override
    public void move() {
        System.out.println("Swimming");
    }
}

```

Теперь подклассы `Sparrow` и `Penguin` дополняют, а не изменяют поведение, и могут быть использованы взаимозаменя

### Принцип разделения интерфейса (ISP)

Этот принцип утверждает, что <mark class="hltr-yellow">лучше иметь несколько узкоспециализированных интерфейсов, чем один "толстый" интерфейс.</mark>

**`Пример`:**

Вместо одного интерфейса:

```java

interface Worker {
    void work();
    void eat();
    void sleep();
}

```

Лучше разделить его на несколько специализированных интерфейсов:

```java

interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

```

Теперь классы могут имплементировать только те интерфейсы, которые им действительно необходимы:

```java

class Employee implements Workable, Eatable {
    @Override
    public void work() {
        System.out.println("Working");
    }

    @Override
    public void eat() {
        System.out.println("Eating");
    }
}

```

### Принцип инверсии зависимостей (DIP)

Этот принцип утверждает, что <mark class="hltr-yellow">высокоуровневые модули не должны зависеть от низкоуровневых модулей. Оба типа модулей должны зависеть от абстракций.</mark>

**`Пример`:**

Плохой пример:

```java

class Keyboard {
    // ...
}

class Computer {
    private Keyboard keyboard;

    public Computer() {
        this.keyboard = new Keyboard();
    }
}

```

Здесь класс `Computer` зависит от конкретного класса `Keyboard`. Вместо этого мы можем ввести абстракцию:

```java

interface Keyboard {
    // ...
}

class MechanicalKeyboard implements Keyboard {
    // ...
}

class Computer {
    private Keyboard keyboard;

    public Computer(Keyboard keyboard) {
        this.keyboard = keyboard;
    }
}

```

Теперь класс `Computer` зависит от абстракции `Keyboard`, и мы можем подставлять любые конкретные реализации `Keyboard`, не изменяя класс `Computer`.
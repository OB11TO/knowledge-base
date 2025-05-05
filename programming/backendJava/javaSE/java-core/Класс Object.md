---
title: Класс Object
tags:
  - JavaSE
related_topics: 
created: 2024-08-30 14:43
modified: 2025-02-12T14:51:54+03:00
difficulty: easy
questions: 
notes: 
links: 
---
Класс `Object` в Java является родительским для всех классов и определяет ряд методов, которые можно переопределить в дочерних классах. Вот список всех методов класса `Object`:

- `equals(Object obj)`: определяет, равен ли данный объект указанному объекту.

Контракт `equals` - это набор правил, которым должен следовать метод `equals`, переопределенный в Java-классе. Этот контракт определяет правильное поведение сравнения объектов на равенство и гарантирует, что метод `equals` работает корректно в различных контекстах.

<mark class="hltr-orange">Основные правила контракта</mark> <mark class="hltr-red">equals</mark>:

1. ==Рефлективность (Reflexivity):==  <mark class="hltr-green2">Объект должен быть равен самому себе.</mark>
    
    ```java
    x.equals(x) должен быть true для всех ненулевых объектов x.
    
    ```
    
2. ==Симметричность (Symmetry):== <mark class="hltr-green2"> Если один объект равен другому, то и другой объект равен первому.</mark>
    
    ```java
    Если x.equals(y) возвращает true, то и y.equals(x) также должен возвращать true.
    
    ```
    
3. ==Транзитивность (Transitivity):== <mark class="hltr-green2"> Если один объект равен второму, и второй равен третьему, то первый также должен быть равен третьему</mark>.
    
    ```java
    Если x.equals(y) и y.equals(z) возвращают true, то и x.equals(z) должен возвращать true.
    
    ```
    
4. ==Консистентность (Consistency):== <mark class="hltr-green2">Результат вызова метода `equals` не должен меняться для объектов, если никакие свойства этих объектов не меняются</mark>.
    
    ```java
    Повторные вызовы x.equals(y) должны возвращать одинаковый результат при неизменных свойствах x и y.
    
    ```
    
5. ==Неравенство== с `null` (Non-nullity): Вызов `equals(null)` ==всегда должен возвращать== `false`.
    
    ```java
    x.equals(null) должен возвращать false для любого ненулевого объекта x.
    
    ```


При переопределении метода `equals` в Java классе следует следовать этому контракту, чтобы обеспечить правильное поведение сравнения объектов на равенство.<mark class="hltr-purple"> В противном случае, сравнение объектов может быть неправильным и привести к неожиданным результатам.</mark>

```java
class MyClass {
    private int value;
    
    // Конструкторы, геттеры и сеттеры
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (!(obj instanceof MyClass)) {
            return false;
        }
        MyClass other = (MyClass) obj;
        return this.value == other.value;
    }
}
```

- `hashCode()`: ==возвращает хеш-код объекта.==

<mark class="hltr-red">Контракт</mark> `hashCode` - это набор правил, которым должен следовать метод `hashCode`, переопределенный в Java-классе.<mark class="hltr-purple"> Этот контракт определяет правила вычисления хэш-кода объекта и гарантирует, что хэш-коды объектов уникально идентифицируют их в различных структурах данных, таких как хэш-таблицы.</mark>

Основные правила контракта `hashCode`:

1. <mark class="hltr-green2">Согласованность (Consistency)</mark>: ==При неизменных свойствах объекта,== вызов `hashCode` ==должен возвращать один и тот же результат на протяжении всего времени выполнения программы.==
    
    ```
    При неизменных свойствах объекта, повторные вызовы x.hashCode() должны возвращать одно и то же значение.
    
    ```
    
2. <mark class="hltr-green2">Согласованность с</mark> `equals` (Consistency with equals): ==Если два объекта равны согласно методу== `equals`, т==о их хэш-коды должны быть равны.==
    
    ```
    Если x.equals(y) возвращает true, то x.hashCode() должен быть равен y.hashCode().
    
    ```
    
3. Необязательность согласованности (Optional consistency): ==Для двух неравных объектов не требуется, чтобы их хэш-коды были обязательно различны.==
    
    ```
    Для неравных объектов x и y, хэш-коды x.hashCode() и y.hashCode() могут быть равными.
    
    ```
    
4. Равенство хэш-кодов (Equality of hash codes): ==Если хэш-коды двух объектов равны, это не обязательно означает, что объекты равны.==
    
    ```
    Для объектов x и y, равенство x.hashCode() == y.hashCode() не гарантирует равенство x.equals(y).
    
    ```
    

Хэш-коды используются в структурах данных, таких как хэш-таблицы, чтобы быстро находить объекты в коллекциях. При переопределении метода `hashCode` в Java классе следует следовать этому контракту, чтобы обеспечить правильное функционирование хэш-таблиц и других структур данных, которые оперируют хэш-кодами объектов. Важно также учитывать, что<mark class="hltr-yellow"> хороший метод</mark> `hashCode` <mark class="hltr-yellow">должен стремиться распределять объекты равномерно по всем возможным значениям хэш-кода, чтобы уменьшить вероятность коллизий (ситуаций, когда разным объектам присваиваются одинаковые хэш-коды).</mark>

```java
class MyClass {
    private int value;
    
    // Конструкторы, геттеры и сеттеры
    
    @Override
    public int hashCode() {
        return Objects.hash(value);
    }
}
```

- `toString()`: <mark class="hltr-green2">возвращает строковое представление объекта.</mark>

```java
class MyClass {
    private int value;
    
    // Конструкторы, геттеры и сеттеры
    
    @Override
    public String toString() {
        return "MyClass{" +
                "value=" + value +
                '}';
    }
}
```

- `getClass()`: <mark class="hltr-green2">возвращает класс объекта.</mark>

```java
MyClass obj = new MyClass();
Class<? extends MyClass> clazz = obj.getClass();
System.out.println(clazz.getName());
```

- `notify()`: <mark class="hltr-green2">пробуждает один ожидающий поток выполнения.</mark>

```java
class MyThread implements Runnable {
    private Object lock;
    
    public MyThread(Object lock) {
        this.lock = lock;
    }
    
    @Override
    public void run() {
        synchronized (lock) {
            System.out.println("Thread waiting...");
            try {
                lock.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Thread woken up!");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Object lock = new Object();
        Thread t1 = new Thread(new MyThread(lock));
        Thread t2 = new Thread(new MyThread(lock));
        t1.start();
        t2.start();
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        synchronized (lock) {
            lock.notify();
        }
    }
}
```

- `notifyAll()`: <mark class="hltr-green2">пробуждает все ожидающие потоки выполнения.</mark>

```java
class MyThread implements Runnable {
    private Object lock;
    
    public MyThread(Object lock) {
        this.lock = lock;
    }
    
    @Override
    public void run() {
        synchronized (lock) {
            System.out.println("Thread waiting...");
            try {
                lock.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Thread woken up!");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Object lock = new Object();
        Thread t1 = new Thread(new MyThread(lock));
        Thread t2 = new Thread(new MyThread(lock));
        t1.start();
        t2.start();
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        synchronized (lock) {
            lock.notifyAll();
        }
    }
}
```

- `wait()`: <mark class="hltr-green2">заставляет поток выполнения ожидать, пока другой поток не вызове</mark>т `notify()` или `notifyAll()`.

```java
class MyThread implements Runnable {
  public void run() {
    synchronized(this) {
      try {
        System.out.println("Thread " + Thread.currentThread().getId() + " is waiting");
        wait();
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      System.out.println("Thread " + Thread.currentThread().getId() + " is running again");
    }
  }
}

public class Main {
  public static void main(String[] args) throws InterruptedException {
    MyThread myThread = new MyThread();
    Thread t1 = new Thread(myThread);
    Thread t2 = new Thread(myThread);
    t1.start();
    t2.start();
    Thread.sleep(1000);
    synchronized(myThread) {
      System.out.println("Sending notification to all threads");
      myThread.notifyAll();
    }
  }
}
```

Большинство из этих методов используется для работы с многопоточностью и не требуют переопределения в большинстве случаев. Однако, `equals()`, `hashCode()` и `toString()` часто переопределяются в дочерних классах для более правильного сравнения, хэширования и отображения объектов.

> [!Question]-  А Зачем переопределять хешкод, если мы используем native метод ?
> Yes! Ведь реализация по умолчанию` hashCode()`, <mark class="hltr-purple">унаследованная от Object, считает все объекты в памяти уникальными!</mark>
> 



------


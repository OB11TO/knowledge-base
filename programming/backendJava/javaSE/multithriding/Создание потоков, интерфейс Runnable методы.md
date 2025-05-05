---
title: Создание потоков, class Thread, методы
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:25
modified: 2024-09-11T12:26:47+03:00
questions: 
notes: 
links: 
---
### Создание потоков, class Thread, методы

- Использовать функциональный интерфейс Runnable

```Java
class Printer implements Runnable
{
@Override
public void run(){
}
System.out.println("Печатаю...");
}

public static void main(String[] args) {
Thread newThread = new Thread(new Printer());
newThread.start();
}
```

При использовании данного интерфейса можно создавать несколько потоков на основе одного объекта, имплементирующего Runnable.

```Java
public static void main(String[] args)
{
Printer printer = new Printer();

Thread thread1 = new Thread(printer);
thread1.start();

Thread thread2 = new Thread(printer);
thread2.start();

Thread thread3 = new Thread(printer);
thread3.start();
}

```


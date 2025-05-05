---
title: DeadLock
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:34
modified: 2024-09-11T12:35:11+03:00
questions: 
notes: 
links: 
---
### DeadLock

### Что такое

Deadlock (в переводе с английского — "тупик") — это ситуация, когда два или более потока взаимодействуют в таком образе, что каждый из них ожидает, что другой поток освободит ресурс, который он сам нуждается для продолжения выполнения, и, следовательно, оба потока остаются заблокированными вечно, без возможности завершения своей работы. Это приводит к замороженному состоянию программы и блокировке выполнения потоков.

Пример Deadlock на Java:

```Java
public class DeadlockExample {
    private static Object lock1 = new Object();
    private static Object lock2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Поток 1: Удерживает lock1...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 1: Ожидает lock2...");
                synchronized (lock2) {
                    System.out.println("Поток 1: Захватил lock2.");
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Поток 2: Удерживает lock2...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 2: Ожидает lock1...");
                synchronized (lock1) {
                    System.out.println("Поток 2: Захватил lock1.");
                }
            }
        });

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

В этом примере есть два потока, каждый из которых пытается получить доступ к двум ресурсам (lock1 и lock2), которые другие потоки могут также попытаться захватить. При выполнении программы поток 1 захватывает lock1 и ожидает lock2, в то время как поток 2 захватывает lock2 и ожидает lock1. Оба потока ожидают друг друга, что приводит к Deadlock, и выполнение программы зависает.

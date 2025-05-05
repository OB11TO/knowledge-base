---
title: Избегание DeadLocks
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:35
modified: 2024-09-11T12:35:28+03:00
questions: 
notes: 
links: 
---
### Избегание DeadLocks

Избегание Deadlock в Java можно достичь, используя правильную стратегию управления блокировками и избегая циклических зависимостей между ресурсами. Давайте рассмотрим два примера:

- **Использование Упорядочивания Ресурсов**: Упорядочивание ресурсов позволяет всем потокам получать доступ к ресурсам в одном и том же порядке, что и другие потоки, тем самым избегая циклических зависимостей.

```Java
public class DeadlockAvoidanceExample {
    private static Object resource1 = new Object();
    private static Object resource2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (resource1) {
                System.out.println("Поток 1: Удерживает resource1...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 1: Ожидает resource2...");
                synchronized (resource2) {
                    System.out.println("Поток 1: Захватил resource2.");
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            synchronized (resource1) {
                System.out.println("Поток 2: Удерживает resource1...");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Поток 2: Попытка получения resource2...");
                synchronized (resource2) {
                    System.out.println("Поток 2: Захватил resource2.");
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

В этом примере оба потока получают доступ к ресурсам (resource1 и resource2) в одном и том же порядке, что предотвращает возможность Deadlock.

- **Использование java.util.concurrent.locks.Lock**: Java предоставляет более гибкие средства управления блокировками с использованием класса `ReentrantLock` из пакета `java.util.concurrent.locks`. Этот класс позволяет легко управлять блокировками и предоставляет методы для избежания Deadlock, такие как `tryLock()` и `lockInterruptibly()`.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class DeadlockAvoidanceWithLockExample {
    private static Lock lock1 = new ReentrantLock();
    private static Lock lock2 = new ReentrantLock();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            lock1.lock();
            System.out.println("Поток 1: Удерживает lock1...");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Поток 1: Ожидает lock2...");
            lock2.lock();
            System.out.println("Поток 1: Захватил lock2.");
            lock1.unlock();
            lock2.unlock();
        });

        Thread thread2 = new Thread(() -> {
            lock1.lock();
            System.out.println("Поток 2: Удерживает lock1...");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Поток 2: Ожидает lock2...");
            lock2.lock();
            System.out.println("Поток 2: Захватил lock2.");
            lock1.unlock();
            lock2.unlock();
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

В этом примере мы используем `ReentrantLock` для управления блокировками. Этот класс позволяет легко управлять блокировками и предотвращать Deadlock с помощью методов блокировки и разблокировки.
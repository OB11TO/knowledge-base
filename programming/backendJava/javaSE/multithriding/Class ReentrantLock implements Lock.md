---
title: Class ReentrantLock implements Lock
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:35
modified: 2024-09-11T12:35:38+03:00
questions: 
notes: 
links: 
---
### Class ReentrantLock implements Lock

- `int getHoldCount()`: Возвращает количество удержаний блокировки текущим потоком.

```Java
Lock lock = new ReentrantLock();
lock.lock();
int holdCount = lock.getHoldCount();
System.out.println("Количество удержаний: " + holdCount); // Выведет "Количество удержаний: 1"
lock.unlock();
```

- `protected Thread getOwner()`: Возвращает поток, владеющий блокировкой в данный момент, или null, если блокировка не занята.

```Java
Lock lock = new ReentrantLock();
lock.lock();
Thread owner = lock.getOwner();
System.out.println("Владелец блокировки: " + owner); // Выведет информацию о текущем потоке
lock.unlock();
```

- `protected Collection<Thread> getQueuedThreads()`: Возвращает коллекцию потоков, которые могут ожидать блокировку.

```Java
Lock lock = new ReentrantLock();
lock.lock();
Collection<Thread> queuedThreads = lock.getQueuedThreads();
System.out.println("Ожидающие потоки: " + queuedThreads); // Выведет информацию о потоках в ожидании
lock.unlock();
```

- `int getQueueLength()`: Возвращает оценку количества потоков, ожидающих получения блокировки.

```Java
Lock lock = new ReentrantLock();
lock.lock();
int queueLength = lock.getQueueLength();
System.out.println("Длина очереди ожидания: " + queueLength); // Выведет "Длина очереди ожидания: 0"
lock.unlock();
```

- `protected Collection<Thread> getWaitingThreads(Condition condition)`: Возвращает коллекцию потоков, ожидающих указанное условие.

```Java
Lock lock = new ReentrantLock();
Condition condition = lock.newCondition();
lock.lock();
Collection<Thread> waitingThreads = lock.getWaitingThreads(condition);
System.out.println("Ожидающие потоки для условия: " + waitingThreads); // Выведет информацию о потоках в ожидании
lock.unlock();
```

- `int getWaitQueueLength(Condition condition)`: Возвращает оценку количества потоков, ожидающих указанное условие.

```Java
Lock lock = new ReentrantLock();
Condition condition = lock.newCondition();
lock.lock();
int waitQueueLength = lock.getWaitQueueLength(condition);
System.out.println("Длина очереди ожидания для условия: " + waitQueueLength); // Выведет "Длина очереди ожидания для условия: 0"
lock.unlock();
```

- `boolean hasQueuedThread(Thread thread)`: Проверяет, ожидает ли указанный поток получение блокировки.

```Java
Lock lock = new ReentrantLock();
lock.lock();
boolean hasQueued = lock.hasQueuedThread(Thread.currentThread());
System.out.println("Текущий поток ожидает блокировку: " + hasQueued); // Выведет "Текущий поток ожидает блокировку: true"
lock.unlock();
```

- `boolean hasQueuedThreads()`: Проверяет, ожидают ли какие-либо потоки получение блокировки.

```Java
Lock lock = new ReentrantLock();
lock.lock();
boolean hasQueuedThreads = lock.hasQueuedThreads();
System.out.println("Ожидают ли потоки блокировку: " + hasQueuedThreads); // Выведет "Ожидают ли пот
```

- `boolean hasWaiters(Condition condition)`: Проверяет, есть ли потоки, ожидающие указанное условие, связанное с блокировкой.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Condition;

public class HasWaitersExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        Condition condition = lock.newCondition();
        
        // Проверяем, есть ли ожидающие потоки по условию
        if (lock.hasWaiters(condition)) {
            // Есть ожидающие потоки
        } else {
            // Нет ожидающих потоков
        }
    }
}
```

- `boolean isFair()`: Возвращает true, если блокировка установлена как справедливая.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class IsFairExample {
    public static void main(String[] args) {
        Lock fairLock = new ReentrantLock(true); // Создаем справедливую блокировку
        Lock nonFairLock = new ReentrantLock(); // Создаем несправедливую блокировку
        
        boolean isFair1 = fairLock.isFair();
        boolean isFair2 = nonFairLock.isFair();
        
        System.out.println("Справедливая блокировка: " + isFair1);
        System.out.println("Несправедливая блокировка: " + isFair2);
    }
}
```

- `boolean isHeldByCurrentThread()`: Проверяет, удерживается ли блокировка текущим потоком.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class IsHeldByCurrentThreadExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        // Проверяем, удерживается ли блокировка текущим потоком
        if (lock.isHeldByCurrentThread()) {
            // Блокировка удерживается текущим потоком
        } else {
            // Блокировка не удерживается текущим потоком
        }
    }
}
```

- `boolean isLocked()`: Проверяет, удерживается ли блокировка каким-либо потоком.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class IsLockedExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        // Проверяем, удерживается ли блокировка
        if (lock.isLocked()) {
            // Блокировка удерживается каким-либо потоком
        } else {
            // Блокировка не удерживается ни одним потоком
        }
    }
}
```

- `void lock()`: Захватывает блокировку.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class LockExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        lock.lock(); // Захватываем блокировку
        
        try {
            // Критическая секция, блокировка захвачена
        } finally {
            lock.unlock(); // Освобождение блокировки
        }
    }
}
```

- `void lockInterruptibly()`: Захватывает блокировку, если текущий поток не был прерван.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class LockInterruptiblyExample {
    public static void main(String[] args) throws InterruptedException {
        Lock lock = new ReentrantLock();

        try {
            lock.lockInterruptibly(); // Попытка захвата блокировки, может быть прервана
            // Критическая секция, блокировка захвачена
        } finally {
            lock.unlock(); // Освобождение блокировки
        }
    }
}
```

- `Condition newCondition()`: Возвращает экземпляр Condition для использования с этой блокировкой.

```Java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class NewConditionExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        Condition condition = lock.newCondition(); // Создание экземпляра Condition

        // Теперь можно использовать condition для ожидания и сигнализации
    }
}
```

- `String toString()`: Возвращает строку, идентифицирующую эту блокировку и ее состояние.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ToStringExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();

        String lockInfo = lock.toString();
        System.out.println(lockInfo);
    }
}
```

- `boolean tryLock()`: Захватывает блокировку, если она не удерживается другим потоком в момент вызова.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class TryLockExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();
        
        if (lock.tryLock()) {
            try {
                // Критическая секция, блокировка захвачена
            } finally {
                lock.unlock(); // Освобождение блокировки
            }
        } else {
            // Блокировка уже удерживается другим потоком
        }
    }
}
```

- `boolean tryLock(long timeout, TimeUnit unit)`: Захватывает блокировку, если она не удерживается другим потоком в течение указанного времени ожидания и текущий поток не был прерван.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.TimeUnit;

public class TryLockWithTimeoutExample {
    public static void main(String[] args) throws InterruptedException {
        Lock lock = new ReentrantLock();
        
        if (lock.tryLock(2, TimeUnit.SECONDS)) {
            try {
                // Критическая секция, блокировка захвачена
            } finally {
                lock.unlock(); // Освобождение блокировки
            }
        } else {
            // Блокировка уже удерживается другим потоком или ожидание истекло
        }
    }
}
```

- `void unlock()`: Освобождает блокировку.

```Java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class UnlockExample {
    public static void main(String[] args) {
        Lock lock = new ReentrantLock();

        try {
            lock.lock(); // Захват блокировки
            // Критическая секция, блокировка захвачена
        } finally {
            lock.unlock(); // Освобождение блокировки
        }
    }
}
---
title: class java.lang.Thread - документация
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:27
modified: 2024-09-11T12:28:15+03:00
questions: 
notes: 
links: 
---
- Наследоваться от Thread
    
    ### class java.lang.Thread - документация
    
    - `static int activeCount()` Возвращает оценку количества активных потоков платформы в текущей группе потоков текущего потока и его подгруппах.
    
    ```Java
    int activeThreads = Thread.activeCount();
    System.out.println("Количество активных потоков: " + activeThreads);
    ```
    
    - `static Thread currentThread()` Возвращает объект Thread для текущего потока.
    
    ```Java
    Thread current = Thread.currentThread();
    System.out.println("Текущий поток: " + current.getName());
    ```
    
    - `static void dumpStack()` Выводит трассировку стека текущего потока в стандартный поток ошибок.
    
    ```Java
    System.out.println("Дамп стека текущего потока:");
    Thread.dumpStack();
    ```
    
    - `static int enumerate(Thread[] tarray)` Копирует в указанный массив все активные потоки платформы в текущей группе потоков текущего потока и его подгруппах.
    
    ```Java
    Thread[] threads = new Thread[Thread.activeCount()];
    Thread.enumerate(threads);
    
    System.out.println("Список активных потоков:");
    for (Thread thread : threads) {
        System.out.println(thread.getName());
    }
    ```
    
    - `static Map<Thread, StackTraceElement[]> getAllStackTraces()` Возвращает карту трассировок стека для всех активных потоков платформы.
    
    ```Java
    Map<Thread, StackTraceElement[]> threadStackTraces = Thread.getAllStackTraces();
    
    for (Map.Entry<Thread, StackTraceElement[]> entry : threadStackTraces.entrySet()) {
        Thread thread = entry.getKey();
        StackTraceElement[] stackTrace = entry.getValue();
        System.out.println("Поток: " + thread.getName());
        for (StackTraceElement element : stackTrace) {
            System.out.println("  " + element.toString());
        }
    }
    ```
    
    - `ClassLoader getContextClassLoader()` Возвращает контекстный ClassLoader для этого потока.
    
    ```Java
    ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
    System.out.println("Контекстный ClassLoader: " + classLoader);
    ```
    
    - `static Thread.UncaughtExceptionHandler getDefaultUncaughtExceptionHandler()` Возвращает обработчик по умолчанию, вызываемый при внезапном завершении потока из-за необработанного исключения.
    
    ```Java
    Thread.UncaughtExceptionHandler defaultHandler = Thread.getDefaultUncaughtExceptionHandler();
    System.out.println("Обработчик по умолчанию: " + defaultHandler);
    ```
    
    - `final String getName()` Возвращает имя этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Имя потока: " + Thread.currentThread().getName());
    });
    myThread.start();
    ```
    
    - `final int getPriority()` Возвращает приоритет этого потока.  
        ВАЖНО! Никто не гарантирует, что поток с более высоким приоритетом запустится первее.  
        
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Приоритет потока: " + Thread.currentThread().getPriority());
    });
    myThread.setPriority(Thread.MAX_PRIORITY); // Установка максимального приоритета
    myThread.start();
    ```
    
    - `StackTraceElement[] getStackTrace()` Возвращает массив элементов трассировки стека, представляющий дамп стека этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        StackTraceElement[] stackTrace = Thread.currentThread().getStackTrace();
        for (StackTraceElement element : stackTrace) {
            System.out.println(element.toString());
        }
    });
    myThread.start();
    ```
    
    - `Thread.State getState()` Возвращает состояние этого потока.
    
    У класса `Thread` в Java есть несколько возможных состояний (states), которые отражают его текущее состояние во время выполнения. Эти состояния определены в перечислении `Thread.State`. Вот список возможных состояний:
    
    1. `NEW`: Поток был создан, но еще не запущен.
    2. `RUNNABLE`: Поток выполняется и готов к выполнению на процессоре.
    3. `BLOCKED`: Поток ожидает блокировку для входа в критическую секцию, которая уже захвачена другим потоком.
    4. `WAITING`: Поток ожидает, пока другой поток выполнит определенное действие. Например, он может ожидать вызова метода `wait()`.
    5. `TIMED_WAITING`: Поток находится в ожидании с тайм-аутом. Это состояние возникает, когда поток ждет в течение определенного времени, после чего он автоматически продолжит выполнение. Например, при вызове метода `sleep()`.
    6. `TERMINATED`: Поток завершил выполнение и больше не активен.
    
    Эти состояния позволяют отслеживать и управлять выполнением потоков в многопоточных приложениях.
    
    ```Java
    Thread myThread = new Thread(() -> {
        Thread.State state = Thread.currentThread().getState();
        System.out.println("Состояние потока: " + state);
    });
    myThread.start();
    ```
    
    - `final ThreadGroup getThreadGroup()` Возвращает группу потоков этого потока или null, если поток завершился.
    
    ```Java
    Thread myThread = new Thread(() -> {
        ThreadGroup group = Thread.currentThread().getThreadGroup();
        System.out.println("Группа потоков: " + group.getName());
    });
    myThread.start();
    ```
    
    - `Thread.UncaughtExceptionHandler getUncaughtExceptionHandler()` Возвращает обработчик, вызываемый при внезапном завершении этого потока из-за необработанного исключения.
    
    ```Java
    Thread myThread = new Thread(() -> {
        Thread.UncaughtExceptionHandler handler = Thread.currentThread().getUncaughtExceptionHandler();
        System.out.println("Обработчик необработанных исключений: " + handler);
    });
    myThread.start();
    ```
    
    - `static boolean holdsLock(Object obj)` Возвращает true только в том случае, если текущий поток удерживает монитор объекта obj.
    - `void interrupt()` Прерывает этот поток.
    
    ```Java
    Thread myThread = new Thread(() -> {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    
    myThread.start();
    
    // Перед тем как завершиться, прерываем поток
    myThread.interrupt();
    ```
    
    - `static boolean interrupted()` Проверяет, был ли текущий поток прерван.
    
    ```Java
    Thread.currentThread().interrupt(); // Устанавливаем флаг прерывания для текущего потока
    
    if (Thread.interrupted()) {
        System.out.println("Поток был прерван.");
    } else {
        System.out.println("Поток не был прерван.");
    }
    ```
    
    - `final boolean isAlive()` Проверяет, жив ли этот поток.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
    });
    
    myThread.start();
    
    if (myThread.isAlive()) {
        System.out.println("Поток живой.");
    } else {
        System.out.println("Поток неактивен.");
    }
    ```
    
    - `final boolean isDaemon()` Проверяет, является ли этот поток демоническим.
    
    ```Java
    Thread daemonThread = new Thread(() -> {
        System.out.println("Демонический поток выполняется...");
    });
    daemonThread.setDaemon(true); // Установка потока как демонического
    daemonThread.start();
    if (daemonThread.isDaemon()) {
        System.out.println("Поток является демоническим.");
    } else {
        System.out.println("Поток не является демоническим.");
    }
    ```
    
    - `boolean isInterrupted()` Проверяет, был ли этот поток прерван.
    
    ```Java
    Thread myThread = new Thread(() -> {
        while (!Thread.currentThread().isInterrupted()) {
            System.out.println("Поток выполняется...");
        }
    });
    myThread.start();
    // Перед тем как завершиться, прерываем поток
    myThread.interrupt();
    ```
    
    - `final boolean isVirtual()` Возвращает true, если этот поток является виртуальным потоком.
    - `final void join()` Ожидает завершения этого потока.
    
    ```Java
    Thread thread1 = new Thread(() -> {
        System.out.println("Поток 1 выполняется...");
    });
    Thread thread2 = new Thread(() -> {
        System.out.println("Поток 2 выполняется...");
    });
    thread1.start();
    thread2.start();
    try {
        thread1.join();
        thread2.join();
    } catch (InterruptedException e) {
        System.out.println("Поток был прерван!");
    }
    System.out.println("Основной поток завершил выполнение.");
    ```
    
    - `final void join(long millis)` Ожидает не более millis миллисекунд завершения этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    myThread.start();
    try {
        myThread.join(3000); // Ожидаем не более 3 секунд
    } catch (InterruptedException e) {
        System.out.println("Основной поток был прерван!");
    }
    ```
    
    - `final void join(long millis, int nanos)` Ожидает не более millis миллисекунд плюс nanos наносекунд завершения этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    myThread.start();
    try {
        myThread.join(3, 500000); // Ожидаем не более 3 секунд и 500 миллисекунд
    } catch (InterruptedException e) {
        System.out.println("Основной поток был прерван!");
    }
    ```
    
    - `final boolean join(Duration duration)` Ожидает завершения этого потока в течение указанной продолжительности.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток выполняется...");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            System.out.println("Поток был прерван!");
        }
    });
    myThread.start();
    try {
        Duration duration = Duration.ofSeconds(3); // Ожидаем не более 3 секунд
        boolean joined = myThread.join(duration);
        if (joined) {
            System.out.println("Поток успешно завершил выполнение.");
        } else {
            System.out.println("Ожидание завершения потока превысило указанный интервал.");
        }
    } catch (InterruptedException e) {
        System.out.println("Основной поток был прерван!");
    }
    ```
    
    - `static Thread.Builder.OfPlatform ofPlatform()` Возвращает билдер для создания платформенного потока или фабрики потоков, создающей платформенные потоки.
    - `static Thread.Builder.OfVirtual ofVirtual()` Возвращает билдер для создания виртуального потока или фабрики потоков, создающей виртуальные потоки.
    - `static void onSpinWait()` Указывает, что вызывающий поток временно не может продвигаться, пока не произойдут одно или несколько действий со стороны других действий.
    
    ```Java
    System.out.println("Начало выполнения программы");
    while (true) {
        // Имитация ожидания
        Thread.onSpinWait();
        System.out.println("Выполнение работы...");
    }
    ```
    
    - `void run()` Этот метод выполняется потоком при его запуске.
    - `void setContextClassLoader(ClassLoader cl)` Устанавливает контекстный ClassLoader для этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        ClassLoader contextClassLoader = Thread.currentThread().getContextClassLoader();
        System.out.println("Контекстный ClassLoader: " + contextClassLoader);
    });
    myThread.start();
    ```
    
    - `final void setDaemon(boolean on)` Помечает этот поток как демонический или недемонический.
    
    ```Java
    Thread daemonThread = new Thread(() -> {
        System.out.println("Демонический поток выполняется...");
    });
    daemonThread.setDaemon(true); // Установка потока как демонического
    daemonThread.start();
    ```
    
    - `static void setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler ueh)` Устанавливает обработчик по умолчанию, вызываемый при внезапном завершении потока из-за необработанного исключения, если для этого потока не был определен другой обработчик.
    
    ```Java
    Thread.setDefaultUncaughtExceptionHandler((thread, throwable) -> {
        System.out.println("Поток " + thread.getName() + " завершился с исключением: " + throwable.getMessage());
    });
    ```
    
    - `final void setName(String name)` Изменяет имя этого потока на заданное имя.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Поток с именем: " + Thread.currentThread().getName());
    });
    myThread.setName("Мой Поток"); // Установка имени потока
    myThread.start();
    ```
    
    - `final void setPriority(int newPriority)` Изменяет приоритет этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Приоритет потока: " + Thread.currentThread().getPriority());
    });
    myThread.setPriority(Thread.MAX_PRIORITY); // Установка максимального приоритета
    myThread.start();
    ```
    
    - `void setUncaughtExceptionHandler(Thread.UncaughtExceptionHandler ueh)` Устанавливает обработчик, вызываемый при внезапном завершении этого потока из-за необработанного исключения.
    
    ```Java
    Thread myThread = new Thread(() -> {
        Thread.currentThread().setUncaughtExceptionHandler((thread, throwable) -> {
            System.out.println("Поток " + thread.getName() + " завершился с исключением: " + throwable.getMessage());
        });
        // Генерируем исключение
        throw new RuntimeException("Исключение в потоке");
    });
    myThread.start();
    ```
    
    - `static void sleep(long millis)` Заставляет текущий выполняющийся поток спать (временно прекратить выполнение) на указанное количество миллисекунд, с учетом точности системных таймеров и планировщиков.
        
        Более того, в методе **sleep** есть автоматическая проверка переменной **isInterrupted**. Если нить вызовет метод **sleep**, то этот метод сначала проверит, а не установлена ли для текущей (вызвавшей его нити) переменная **isInterrupted** в true. И если установлена, то метод не будет спать, а выкинет исключение **InterruptedException**.
        
    
    ```Java
    public class SleepExample {
        public static void main(String[] args) {
            System.out.println("Начало выполнения программы");
    
            try {
                // Приостанавливаем выполнение текущего потока на 3 секунды
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                // Обработка исключения, если поток был прерван
                e.printStackTrace();
            }
    
            System.out.println("Завершение программы");
        }
    }
    ```
    
    - `static void sleep(long millis, int nanos)` Заставляет текущий выполняющийся поток спать на указанное количество миллисекунд и наносекунд, с учетом точности системных таймеров и планировщиков.
        
        ```Java
        System.out.println("Начало выполнения программы");
        
        try {
            // Приостанавливаем выполнение текущего потока на 2 секунды и 500 миллисекунд
            Thread.sleep(2, 500000);
        } catch (InterruptedException e) {
            // Обработка исключения, если поток был прерван
            e.printStackTrace();
        }
        
        System.out.println("Завершение программы");
        ```
        
    - `static void sleep(Duration duration)` Заставляет текущий выполняющийся поток спать на указанную продолжительность, с учетом точности системных таймеров и планировщиков.
        
        ```Java
        import java.time.Duration;
        
        public class SleepExample {
            public static void main(String[] args) {
                System.out.println("Начало выполнения программы");
        
                try {
                    Duration duration = Duration.ofSeconds(3); // Создаем объект Duration на 3 секунды
                    Thread.sleep(duration); // Засыпаем на указанное количество времени
                } catch (InterruptedException e) {
                    // Обработка исключения, если поток был прерван
                    e.printStackTrace();
                }
        
                System.out.println("Завершение программы");
            }
        }
        ```
        
    - `void start()` Планирует начало выполнения этого потока.
    - `static Thread startVirtualThread(Runnable task)` Создает виртуальный поток для выполнения задачи и планирует его выполнение.
    - `final long threadId()` Возвращает идентификатор этого потока.
    
    ```Java
    long threadId = Thread.currentThread().getId();
    System.out.println("Идентификатор текущего потока: " + threadId);
    ```
    
    - `String toString()` Возвращает строковое представление этого потока.
    
    ```Java
    Thread myThread = new Thread(() -> {
        System.out.println("Строковое представление потока: " + Thread.currentThread());
    });
    myThread.start();
    ```
    
    - `static void yield()`Подсказка планировщику, что текущий поток готов уступить использование процессора.
    
    ```Java
    Thread thread1 = new Thread(() -> {
        for (int i = 0; i < 5; i++) {
            System.out.println("Поток 1 выполняется...");
            Thread.yield(); // Уступаем процессорное время
        }
    });
    
    Thread thread2 = new Thread(() -> {
        for (int i = 0; i < 5; i++) {
            System.out.println("Поток 2 выполняется...");
            Thread.yield(); // Уступаем процессорное время
        }
    });
    
    thread1.start();
    thread2.start();
    ```
    

  

```Java
class Printer extends Thread
{
@Override
public void run(){
}
System.out.println("Печатаю...");
}

public static void main(String[] args) {
Printer printer = new Printer();
printer.start();
}
```

У такого решения есть минусы :

- Вам может понадобиться запустить несколько нитей на основе одного единственного объекта, как это сделано в примере с 3 потоками выше.
- Если вы унаследовались от класса Thread, вы не можете добавить еще один класс-родитель к своему классу.
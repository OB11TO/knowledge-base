---
title: Примеры профилирования на java
tags:
  - JavaSE
related_topics: 
created: 2025-01-31 11:51
modified: 2025-01-31T14:35:49+03:00
questions: 
notes: 
links: 
---


## 🔥 Часть 1: CPU-профилирование (анализ нагрузки на процессор)

### 🎯 Сценарий: Неоптимальный алгоритм

Мы создадим программу, которая выполняет **неэффективные вычисления** и потребляет много CPU. Ты запустишь её, а потом посмотрим в **VisualVM**, какие методы загружают процессор.
### 📜 Код:

Скопируй и запусти этот код в **IntelliJ IDEA** или через консоль:

```java
import java.util.Random;

public class CpuProfilingExample {
    public static void main(String[] args) {
        System.out.println("Запускаем CPU-нагрузку...");
        while (true) {
            performHeavyCalculation();
        }
    }

    private static void performHeavyCalculation() {
        Random random = new Random();
        double result = 0;
        for (int i = 0; i < 100_000; i++) {
            result += Math.sqrt(random.nextDouble()) * Math.pow(random.nextDouble(), 2);
        }
    }
}
```

### 🚀 Шаги:

1. Скомпилируй и запусти этот код.
2. Открой **VisualVM** (введи в консоли `jvisualvm`, если у тебя JDK установлен).
3. Найди свой процесс (обычно **CpuProfilingExample**).
4. Перейди на вкладку **Profiler** и нажми **CPU**.
5. Дождись сбора данных и посмотри, **какие методы загружают процессор**.

### 🔍 Что мы увидим?

- **Метод `performHeavyCalculation()` будет самым затратным**.
- В профайлере ты увидишь **Math.sqrt() и Math.pow() как самые тяжелые операции**.
- ЦП будет загружен на **100%**, потому что код работает в бесконечном цикле.



![[Pasted image 20250131140220.png]]
![[Pasted image 20250131140232.png]]



### 💡 Почему это важно?

Это классический пример, когда неоптимальный код нагружает процессор. В реальных приложениях это может быть **плохо спроектированный алгоритм или неоптимальная работа с коллекциями**.



----

## 🔥 Часть 2: Memory-профилирование (анализ утечек памяти)

### 🎯 Сценарий: Утечка памяти из-за неправильного использования коллекций

Мы создадим программу, которая **накапливает объекты в памяти и не освобождает их**. Твоя задача — запустить её, посмотреть в **VisualVM**, как растет потребление памяти, а затем проанализировать **Heap Dump**.
### 📜 Код:

```java
import java.util.ArrayList;
import java.util.List;

public class MemoryLeakExample {
    private static final List<byte[]> memoryLeakList = new ArrayList<>();

    public static void main(String[] args) {
        System.out.println("Запускаем утечку памяти...");
        while (true) {
            memoryLeakList.add(new byte[1_000_000]); // 1 MB данных
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

### 🚀 Шаги:

1. Запусти код.
2. Открой **VisualVM** и найди свой процесс.
3. Перейди на вкладку **Monitor** и **смотри, как растет потребление памяти**.
4. Сделай **Heap Dump** (вкладка Sampler → Heap Dump).
5. Проанализируй, какие объекты занимают больше всего места.

### 🔍 Что мы увидим?

- График памяти **будет постоянно расти** (мы не удаляем объекты).
- В Heap Dump будет **огромное количество `byte[]`**, хранящихся в `memoryLeakList`.
- Через несколько минут программа **упадет с `OutOfMemoryError`**.

### 💡 Почему это важно?

В реальных приложениях такое может случаться, если:

- Мы **забыли очищать коллекции** (например, кеши).
- Мы **держим ссылки на ненужные объекты**, не давая GC их удалить.
- Объекты **неправильно управляются в пулах или синглтонах**.

-----

## 🔥 Часть 3: Thread-профилирование (поиск блокировок и дедлоков)

### 🎯 Сценарий: Потоки зависают в дедлоке

Мы создадим программу, в которой **два потока попытаются захватить ресурсы в обратном порядке**, вызывая **Deadlock**.

### 📜 Код:
```java
public class DeadlockExample {
    private static final Object LOCK1 = new Object();
    private static final Object LOCK2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (LOCK1) {
                System.out.println("Thread 1: Захватил LOCK1");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                synchronized (LOCK2) {
                    System.out.println("Thread 1: Захватил LOCK2");
                }
            }
        });

        Thread thread2 = new Thread(() -> {
            synchronized (LOCK2) {
                System.out.println("Thread 2: Захватил LOCK2");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                synchronized (LOCK1) {
                    System.out.println("Thread 2: Захватил LOCK1");
                }
            }
        });

        thread1.start();
        thread2.start();
    }
}

```


### 🚀 Шаги:

1. Запусти код.
2. Открой **VisualVM** → вкладка **Threads**.
3. Найди **Deadlock** — VisualVM покажет зависшие потоки.
4. Выполни `jstack <PID>` в консоли и найди блокировки.

![[Pasted image 20250131140454.png]]
```java

2025-01-31 14:04:28
Full thread dump OpenJDK 64-Bit Server VM (17.0.10+7 mixed mode, sharing):

Threads class SMR info:
_java_thread_list=0x000075744c003280, length=22, elements={
0x000075755414e0c0, 0x000075755414f4b0, 0x0000757554155d20, 0x00007575541570e0,
0x0000757554158500, 0x0000757554159ec0, 0x000075755415b400, 0x0000757554164870,
0x0000757554170300, 0x0000757554257400, 0x0000757554259db0, 0x000075755425daf0,
0x000075755425eb10, 0x0000757554027bd0, 0x00007574b0000eb0, 0x0000757468063540,
0x00007574680e6020, 0x0000757468144a50, 0x000075744c0017b0, 0x0000757444010fd0,
0x0000757444016b10, 0x000075744c002880
}

"Reference Handler" #2 daemon prio=10 os_prio=0 cpu=0,55ms elapsed=21,38s tid=0x000075755414e0c0 nid=0x2ebc waiting on condition  [0x000075752d127000]
   java.lang.Thread.State: RUNNABLE
        at java.lang.ref.Reference.waitForReferencePendingList(java.base@17.0.10/Native Method)
        at java.lang.ref.Reference.processPendingReferences(java.base@17.0.10/Reference.java:253)
        at java.lang.ref.Reference$ReferenceHandler.run(java.base@17.0.10/Reference.java:215)

   Locked ownable synchronizers:
        - None

"Finalizer" #3 daemon prio=8 os_prio=0 cpu=0,18ms elapsed=21,38s tid=0x000075755414f4b0 nid=0x2ebd in Object.wait()  [0x000075752d027000]
   java.lang.Thread.State: WAITING (on object monitor)
        at java.lang.Object.wait(java.base@17.0.10/Native Method)
        - waiting on <0x000000062a408178> (a java.lang.ref.ReferenceQueue$Lock)
        at java.lang.ref.ReferenceQueue.remove(java.base@17.0.10/ReferenceQueue.java:155)
        - locked <0x000000062a408178> (a java.lang.ref.ReferenceQueue$Lock)
        at java.lang.ref.ReferenceQueue.remove(java.base@17.0.10/ReferenceQueue.java:176)
        at java.lang.ref.Finalizer$FinalizerThread.run(java.base@17.0.10/Finalizer.java:172)

   Locked ownable synchronizers:
        - None

"Signal Dispatcher" #4 daemon prio=9 os_prio=0 cpu=0,34ms elapsed=21,37s tid=0x0000757554155d20 nid=0x2ec6 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"Service Thread" #5 daemon prio=9 os_prio=0 cpu=0,56ms elapsed=21,37s tid=0x00007575541570e0 nid=0x2ec7 runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"Monitor Deflation Thread" #6 daemon prio=9 os_prio=0 cpu=2,49ms elapsed=21,37s tid=0x0000757554158500 nid=0x2ec8 runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"C2 CompilerThread0" #7 daemon prio=9 os_prio=0 cpu=929,09ms elapsed=21,37s tid=0x0000757554159ec0 nid=0x2ec9 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   No compile task

   Locked ownable synchronizers:
        - None

"C1 CompilerThread0" #10 daemon prio=9 os_prio=0 cpu=557,50ms elapsed=21,37s tid=0x000075755415b400 nid=0x2eca waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   No compile task

   Locked ownable synchronizers:
        - None

"Sweeper thread" #11 daemon prio=9 os_prio=0 cpu=1,29ms elapsed=21,37s tid=0x0000757554164870 nid=0x2ecb runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"Common-Cleaner" #12 daemon prio=8 os_prio=0 cpu=0,66ms elapsed=21,36s tid=0x0000757554170300 nid=0x2ed8 in Object.wait()  [0x000075752c2fe000]
   java.lang.Thread.State: TIMED_WAITING (on object monitor)
        at java.lang.Object.wait(java.base@17.0.10/Native Method)
        - waiting on <0x000000062a428178> (a java.lang.ref.ReferenceQueue$Lock)
        at java.lang.ref.ReferenceQueue.remove(java.base@17.0.10/ReferenceQueue.java:155)
        - locked <0x000000062a428178> (a java.lang.ref.ReferenceQueue$Lock)
        at jdk.internal.ref.CleanerImpl.run(java.base@17.0.10/CleanerImpl.java:140)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)
        at jdk.internal.misc.InnocuousThread.run(java.base@17.0.10/InnocuousThread.java:162)

   Locked ownable synchronizers:
        - None

"Monitor Ctrl-Break" #13 daemon prio=5 os_prio=0 cpu=11,15ms elapsed=21,31s tid=0x0000757554257400 nid=0x2efd runnable  [0x000075752c1fe000]
   java.lang.Thread.State: RUNNABLE
        at sun.nio.ch.SocketDispatcher.read0(java.base@17.0.10/Native Method)
        at sun.nio.ch.SocketDispatcher.read(java.base@17.0.10/SocketDispatcher.java:47)
        at sun.nio.ch.NioSocketImpl.tryRead(java.base@17.0.10/NioSocketImpl.java:266)
        at sun.nio.ch.NioSocketImpl.implRead(java.base@17.0.10/NioSocketImpl.java:317)
        at sun.nio.ch.NioSocketImpl.read(java.base@17.0.10/NioSocketImpl.java:355)
        at sun.nio.ch.NioSocketImpl$1.read(java.base@17.0.10/NioSocketImpl.java:808)
        at java.net.Socket$SocketInputStream.read(java.base@17.0.10/Unknown Source)
        at sun.nio.cs.StreamDecoder.readBytes(java.base@17.0.10/StreamDecoder.java:270)
        at sun.nio.cs.StreamDecoder.implRead(java.base@17.0.10/StreamDecoder.java:313)
        at sun.nio.cs.StreamDecoder.read(java.base@17.0.10/StreamDecoder.java:188)
        - locked <0x000000062a436578> (a java.io.InputStreamReader)
        at java.io.InputStreamReader.read(java.base@17.0.10/InputStreamReader.java:177)
        at java.io.BufferedReader.fill(java.base@17.0.10/BufferedReader.java:162)
        at java.io.BufferedReader.readLine(java.base@17.0.10/BufferedReader.java:329)
        - locked <0x000000062a436578> (a java.io.InputStreamReader)
        at java.io.BufferedReader.readLine(java.base@17.0.10/BufferedReader.java:396)
        at com.intellij.rt.execution.application.AppMainV2$1.run(AppMainV2.java:54)

   Locked ownable synchronizers:
        - <0x000000062a5b5fa8> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)

"Notification Thread" #14 daemon prio=9 os_prio=0 cpu=0,09ms elapsed=21,31s tid=0x0000757554259db0 nid=0x2efe runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"Thread-0" #15 prio=5 os_prio=0 cpu=1,25ms elapsed=21,31s tid=0x000075755425daf0 nid=0x2f01 waiting for monitor entry  [0x0000757525efd000]
   java.lang.Thread.State: BLOCKED (on object monitor)
        at ru.ob11to.memory.proftest.DeadlockExample.lambda$main$0(DeadlockExample.java:17)
        - waiting to lock <0x000000062a4181a0> (a java.lang.Object)
        - locked <0x000000062a410328> (a java.lang.Object)
        at ru.ob11to.memory.proftest.DeadlockExample$$Lambda$14/0x00007574d0001208.run(Unknown Source)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - None

"Thread-1" #16 prio=5 os_prio=0 cpu=0,94ms elapsed=21,31s tid=0x000075755425eb10 nid=0x2f02 waiting for monitor entry  [0x0000757525dfd000]
   java.lang.Thread.State: BLOCKED (on object monitor)
        at ru.ob11to.memory.proftest.DeadlockExample.lambda$main$1(DeadlockExample.java:31)
        - waiting to lock <0x000000062a410328> (a java.lang.Object)
        - locked <0x000000062a4181a0> (a java.lang.Object)
        at ru.ob11to.memory.proftest.DeadlockExample$$Lambda$15/0x00007574d0001428.run(Unknown Source)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - None

"DestroyJavaVM" #17 prio=5 os_prio=0 cpu=78,41ms elapsed=21,31s tid=0x0000757554027bd0 nid=0x2e93 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"Attach Listener" #18 daemon prio=9 os_prio=0 cpu=392,13ms elapsed=20,33s tid=0x00007574b0000eb0 nid=0x2f41 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"JFR Recorder Thread" #19 daemon prio=5 os_prio=0 cpu=0,09ms elapsed=17,36s tid=0x0000757468063540 nid=0x2f44 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

   Locked ownable synchronizers:
        - None

"JFR Periodic Tasks" #20 daemon prio=9 os_prio=0 cpu=93,78ms elapsed=17,13s tid=0x00007574680e6020 nid=0x2f47 in Object.wait()  [0x0000757524a61000]
   java.lang.Thread.State: TIMED_WAITING (on object monitor)
        at java.lang.Object.wait(java.base@17.0.10/Native Method)
        - waiting on <no object reference available>
        at jdk.jfr.internal.PlatformRecorder.takeNap(jdk.jfr@17.0.10/PlatformRecorder.java:544)
        - locked <0x000000062a420258> (a jdk.jfr.internal.JVM$ChunkRotationMonitor)
        at jdk.jfr.internal.PlatformRecorder.periodicTask(jdk.jfr@17.0.10/PlatformRecorder.java:524)
        at jdk.jfr.internal.PlatformRecorder.lambda$startDiskMonitor$1(jdk.jfr@17.0.10/PlatformRecorder.java:449)
        at jdk.jfr.internal.PlatformRecorder$$Lambda$75/0x00007574d0054868.run(jdk.jfr@17.0.10/Unknown Source)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - None

"RMI TCP Accept-0" #23 daemon prio=9 os_prio=0 cpu=0,82ms elapsed=16,95s tid=0x0000757468144a50 nid=0x2f48 runnable  [0x0000757524961000]
   java.lang.Thread.State: RUNNABLE
        at sun.nio.ch.Net.accept(java.base@17.0.10/Native Method)
        at sun.nio.ch.NioSocketImpl.accept(java.base@17.0.10/NioSocketImpl.java:760)
        at java.net.ServerSocket.implAccept(java.base@17.0.10/ServerSocket.java:675)
        at java.net.ServerSocket.platformImplAccept(java.base@17.0.10/ServerSocket.java:641)
        at java.net.ServerSocket.implAccept(java.base@17.0.10/ServerSocket.java:617)
        at java.net.ServerSocket.implAccept(java.base@17.0.10/ServerSocket.java:574)
        at java.net.ServerSocket.accept(java.base@17.0.10/ServerSocket.java:532)
        at sun.management.jmxremote.LocalRMIServerSocketFactory$1.accept(jdk.management.agent@17.0.10/LocalRMIServerSocketFactory.java:52)
        at sun.rmi.transport.tcp.TCPTransport$AcceptLoop.executeAcceptLoop(java.rmi@17.0.10/TCPTransport.java:413)
        at sun.rmi.transport.tcp.TCPTransport$AcceptLoop.run(java.rmi@17.0.10/TCPTransport.java:377)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - <0x000000062a5dfc78> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)

"RMI TCP Connection(1)-127.0.0.1" #24 daemon prio=9 os_prio=0 cpu=212,16ms elapsed=16,94s tid=0x000075744c0017b0 nid=0x2f4a runnable  [0x0000757524860000]
   java.lang.Thread.State: RUNNABLE
        at sun.nio.ch.Net.poll(java.base@17.0.10/Native Method)
        at sun.nio.ch.NioSocketImpl.park(java.base@17.0.10/NioSocketImpl.java:186)
        at sun.nio.ch.NioSocketImpl.timedRead(java.base@17.0.10/NioSocketImpl.java:290)
        at sun.nio.ch.NioSocketImpl.implRead(java.base@17.0.10/NioSocketImpl.java:314)
        at sun.nio.ch.NioSocketImpl.read(java.base@17.0.10/NioSocketImpl.java:355)
        at sun.nio.ch.NioSocketImpl$1.read(java.base@17.0.10/NioSocketImpl.java:808)
        at java.net.Socket$SocketInputStream.read(java.base@17.0.10/Socket.java:966)
        at java.io.BufferedInputStream.fill(java.base@17.0.10/BufferedInputStream.java:244)
        at java.io.BufferedInputStream.read(java.base@17.0.10/BufferedInputStream.java:263)
        - locked <0x000000062a584670> (a java.io.BufferedInputStream)
        at java.io.FilterInputStream.read(java.base@17.0.10/FilterInputStream.java:82)
        at sun.rmi.transport.tcp.TCPTransport.handleMessages(java.rmi@17.0.10/TCPTransport.java:569)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(java.rmi@17.0.10/TCPTransport.java:828)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(java.rmi@17.0.10/TCPTransport.java:705)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$$Lambda$148/0x00007574d00fe640.run(java.rmi@17.0.10/Unknown Source)
        at java.security.AccessController.executePrivileged(java.base@17.0.10/AccessController.java:776)
        at java.security.AccessController.doPrivileged(java.base@17.0.10/AccessController.java:399)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(java.rmi@17.0.10/TCPTransport.java:704)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(java.base@17.0.10/ThreadPoolExecutor.java:1136)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(java.base@17.0.10/ThreadPoolExecutor.java:635)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - <0x000000062a5dc7f0> (a java.util.concurrent.ThreadPoolExecutor$Worker)
        - <0x000000062a5dcc48> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)

"RMI Scheduler(0)" #25 daemon prio=9 os_prio=0 cpu=0,35ms elapsed=16,93s tid=0x0000757444010fd0 nid=0x2f4b waiting on condition  [0x0000757524761000]
   java.lang.Thread.State: TIMED_WAITING (parking)
        at jdk.internal.misc.Unsafe.park(java.base@17.0.10/Native Method)
        - parking to wait for  <0x000000062a418428> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
        at java.util.concurrent.locks.LockSupport.parkNanos(java.base@17.0.10/LockSupport.java:252)
        at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(java.base@17.0.10/AbstractQueuedSynchronizer.java:1672)
        at java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(java.base@17.0.10/ScheduledThreadPoolExecutor.java:1182)
        at java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(java.base@17.0.10/ScheduledThreadPoolExecutor.java:899)
        at java.util.concurrent.ThreadPoolExecutor.getTask(java.base@17.0.10/ThreadPoolExecutor.java:1062)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(java.base@17.0.10/ThreadPoolExecutor.java:1122)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(java.base@17.0.10/ThreadPoolExecutor.java:635)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - None

"JMX server connection timeout 26" #26 daemon prio=9 os_prio=0 cpu=8,92ms elapsed=16,93s tid=0x0000757444016b10 nid=0x2f4c in Object.wait()  [0x0000757524661000]
   java.lang.Thread.State: TIMED_WAITING (on object monitor)
        at java.lang.Object.wait(java.base@17.0.10/Native Method)
        - waiting on <no object reference available>
        at com.sun.jmx.remote.internal.ServerCommunicatorAdmin$Timeout.run(java.management@17.0.10/ServerCommunicatorAdmin.java:171)
        - locked <0x000000062a408328> (a [I)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - None

"RMI TCP Connection(2)-127.0.0.1" #27 daemon prio=9 os_prio=0 cpu=1,76ms elapsed=15,88s tid=0x000075744c002880 nid=0x2f4f runnable  [0x0000757524c60000]
   java.lang.Thread.State: RUNNABLE
        at sun.nio.ch.Net.poll(java.base@17.0.10/Native Method)
        at sun.nio.ch.NioSocketImpl.park(java.base@17.0.10/NioSocketImpl.java:186)
        at sun.nio.ch.NioSocketImpl.timedRead(java.base@17.0.10/NioSocketImpl.java:290)
        at sun.nio.ch.NioSocketImpl.implRead(java.base@17.0.10/NioSocketImpl.java:314)
        at sun.nio.ch.NioSocketImpl.read(java.base@17.0.10/NioSocketImpl.java:355)
        at sun.nio.ch.NioSocketImpl$1.read(java.base@17.0.10/NioSocketImpl.java:808)
        at java.net.Socket$SocketInputStream.read(java.base@17.0.10/Socket.java:966)
        at java.io.BufferedInputStream.fill(java.base@17.0.10/BufferedInputStream.java:244)
        at java.io.BufferedInputStream.read(java.base@17.0.10/BufferedInputStream.java:263)
        - locked <0x000000062bc00130> (a java.io.BufferedInputStream)
        at java.io.FilterInputStream.read(java.base@17.0.10/FilterInputStream.java:82)
        at sun.rmi.transport.tcp.TCPTransport.handleMessages(java.rmi@17.0.10/TCPTransport.java:569)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(java.rmi@17.0.10/TCPTransport.java:828)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(java.rmi@17.0.10/TCPTransport.java:705)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$$Lambda$148/0x00007574d00fe640.run(java.rmi@17.0.10/Unknown Source)
        at java.security.AccessController.executePrivileged(java.base@17.0.10/AccessController.java:776)
        at java.security.AccessController.doPrivileged(java.base@17.0.10/AccessController.java:399)
        at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(java.rmi@17.0.10/TCPTransport.java:704)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(java.base@17.0.10/ThreadPoolExecutor.java:1136)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(java.base@17.0.10/ThreadPoolExecutor.java:635)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

   Locked ownable synchronizers:
        - <0x000000062a4285b0> (a java.util.concurrent.ThreadPoolExecutor$Worker)
        - <0x000000062a5dc650> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)

"VM Thread" os_prio=0 cpu=11,23ms elapsed=21,38s tid=0x000075755414a000 nid=0x2eba runnable  

"GC Thread#0" os_prio=0 cpu=2,23ms elapsed=21,39s tid=0x0000757554055940 nid=0x2ea1 runnable  

"GC Thread#1" os_prio=0 cpu=2,20ms elapsed=15,88s tid=0x00007574c400a4b0 nid=0x2f50 runnable  

"GC Thread#2" os_prio=0 cpu=2,25ms elapsed=15,88s tid=0x00007574c400abd0 nid=0x2f51 runnable  

"GC Thread#3" os_prio=0 cpu=2,22ms elapsed=15,88s tid=0x00007574c400b420 nid=0x2f52 runnable  

"GC Thread#4" os_prio=0 cpu=0,47ms elapsed=15,88s tid=0x00007574c400be30 nid=0x2f53 runnable  

"GC Thread#5" os_prio=0 cpu=2,24ms elapsed=15,88s tid=0x00007574c400c840 nid=0x2f54 runnable  

"GC Thread#6" os_prio=0 cpu=2,24ms elapsed=15,88s tid=0x00007574c400d640 nid=0x2f55 runnable  

"GC Thread#7" os_prio=0 cpu=2,23ms elapsed=15,88s tid=0x00007574c400e440 nid=0x2f56 runnable  

"G1 Main Marker" os_prio=0 cpu=0,06ms elapsed=21,39s tid=0x00007575540668e0 nid=0x2ea2 runnable  

"G1 Conc#0" os_prio=0 cpu=0,04ms elapsed=21,39s tid=0x0000757554067850 nid=0x2ea3 runnable  

"G1 Refine#0" os_prio=0 cpu=0,06ms elapsed=21,39s tid=0x000075755411c490 nid=0x2ea6 runnable  

"G1 Service" os_prio=0 cpu=4,30ms elapsed=21,39s tid=0x000075755411d390 nid=0x2ea7 runnable  

"VM Periodic Task Thread" os_prio=0 cpu=15,81ms elapsed=21,31s tid=0x000075755402a460 nid=0x2eff waiting on condition  

JNI global refs: 30, weak refs: 0


Found one Java-level deadlock:
=============================
"Thread-0":
  waiting to lock monitor 0x00007574740012f0 (object 0x000000062a4181a0, a java.lang.Object),
  which is held by "Thread-1"

"Thread-1":
  waiting to lock monitor 0x00007574700010f0 (object 0x000000062a410328, a java.lang.Object),
  which is held by "Thread-0"

Java stack information for the threads listed above:
===================================================
"Thread-0":
        at ru.ob11to.memory.proftest.DeadlockExample.lambda$main$0(DeadlockExample.java:17)
        - waiting to lock <0x000000062a4181a0> (a java.lang.Object)
        - locked <0x000000062a410328> (a java.lang.Object)
        at ru.ob11to.memory.proftest.DeadlockExample$$Lambda$14/0x00007574d0001208.run(Unknown Source)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)
"Thread-1":
        at ru.ob11to.memory.proftest.DeadlockExample.lambda$main$1(DeadlockExample.java:31)
        - waiting to lock <0x000000062a410328> (a java.lang.Object)
        - locked <0x000000062a4181a0> (a java.lang.Object)
        at ru.ob11to.memory.proftest.DeadlockExample$$Lambda$15/0x00007574d0001428.run(Unknown Source)
        at java.lang.Thread.run(java.base@17.0.10/Thread.java:840)

Found 1 deadlock.


```
![[Pasted image 20250131140729.png]]


### 🔍 Что мы увидим?

- **Оба потока зависнут**, потому что один держит `LOCK1`, а другой `LOCK2`.
- **VisualVM покажет DEADLOCK!**
- `jstack` покажет стек вызова с пометкой `Deadlock detected`.

### 💡 Почему это важно?

Deadlock’и — это **одна из главных проблем многопоточных программ**. Они могут приводить к зависанию всей системы

----

## 🔥 Часть 4: GC-профилирование (анализ сборки мусора)

### 🎯 Сценарий: Высокая нагрузка на GC

Создадим код, который **создает миллионы короткоживущих объектов**, вызывая частый запуск GC.

### 📜 Код:
```java
import java.util.ArrayList;
import java.util.List;

public class GcPressureExample {
    public static void main(String[] args) {
        System.out.println("Создаем нагрузку на GC...");
        while (true) {
            List<Integer> list = new ArrayList<>();
            for (int i = 0; i < 100_000; i++) {
                list.add(i);
            }
        }
    }
}
```

### 🚀 Шаги:

1. Запусти код.
2. Открой **VisualVM** → вкладка **Monitor** → "GC Activity".
3. Посмотри, как **GC работает очень часто**.
4. Включи GC-логи:
```java
java -Xlog:gc* GcPressureExample
```

### 🔍 Что мы увидим?

- **Частые запуски GC** → приложение тратит много времени на уборку памяти.
- **Проблема "GC Thrashing"** — программа больше собирает мусор, чем выполняет полезную работу.
![[Pasted image 20250131143420.png]]
![[Pasted image 20250131143436.png]]
![[Pasted image 20250131143500.png]]
### 💡 Почему это важно?

Оптимизация GC — ключевой навык Java-разработчика! Если GC работает слишком часто, это **замедляет приложение**.

![[Pasted image 20250131143539.png]]
![[Pasted image 20250131143556.png]]

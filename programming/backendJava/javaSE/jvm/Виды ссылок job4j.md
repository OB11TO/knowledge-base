---
title: Виды ссылок job4j
tags:
  - JVM
  - Job4j
related_topics: 
created: 2025-02-04 16:38
modified: 2025-02-05T14:45:40+03:00
questions: 
notes: 
links: 
---


-----
Ранее мы говорили о том, что <mark class="hltr-yellow">перед сборкой мусора сборщик мусора маркирует объекты</mark>.
<mark class="hltr-green2">В зависимости от этого он решает какие нужно удалить, а какие нет.</mark> На принятие решения об удалении влияет тип ссылки объекта.
Рассмотрим типы ссылок Java.

---
### **1. Strong Reference**

<mark class="hltr-red">Сильная ссылка является "обычной" ссылкой, которую мы с вами привыкли создавать</mark>. При данном типе ссылок <mark class="hltr-purple">объекты удаляются только если на них нет сильной ссылки или они находятся в составе объекта на который нет ссылки.</mark>
Например,
```java
package ru.job4j.gc.ref;

import java.util.concurrent.TimeUnit;

public class StrongDemo {

    public static void main(String[] args) throws InterruptedException {
        example1();
        /* example2(); */
    }

    private static void example1() throws InterruptedException {
        Object[] objects = new Object[100];
        for (int i = 0; i < 100; i++) {
            objects[i] = new Object() {
                @Override
                protected void finalize() throws Throwable {
                    System.out.println("Object removed!");
                }
            };
        }
        for (int i = 0; i < 100; i++) {
            objects[i] = null;
        }
        System.gc();
        TimeUnit.SECONDS.sleep(5);
    }

    private static void example2() throws InterruptedException {
        Object[] objects = new Object[100];
        for (int i = 0; i < 100; i++) {
            Object object = new Object() {
                Object innerObject = new Object() {
                    @Override
                    protected void finalize() throws Throwable {
                        System.out.println("Remove inner object!");
                    }
                };
            };
            objects[i] = object;
        }
        for (int i = 0; i < 100; i++) {
            objects[i] = null;
        }
        System.gc();
        TimeUnit.SECONDS.sleep(5);
    }
}
```

<mark class="hltr-yellow">В первом методе, мы создаем объект и далее их за'null'яем. Вызываем сборщик мусора и ждем некоторое время. Объекты удаляются, т.к. ссылки на них null</mark>
<mark class="hltr-purple">Во втором методе, мы создаем объекты вместе с вложенными. Удаляя внешние объекты как в примере выше удаляются и вложенные, не смотря на то что они не null.</mark>

<mark class="hltr-red">Проблема данного типа ссылок является то, что если в программе есть неиспользуемые ссылки на созданные объекты, то они не будут удалены, что приведет к утечке памяти, что в свою очередь может привести к ошибке OutOfMemoryException - ситуации когда программе не хватает выделенной памяти.</mark>

К примеру такой код явно приведет к этой ошибке. Чтобы быстрее ее увидеть можно выставить аргументы -Xmx4m -Xms4m
```java
private static void example3() {
    List<String> strings = new ArrayList<>();
    while (true) {
        strings.add(String.valueOf(System.currentTimeMillis()));
    }
}
```

![[Pasted image 20250204165126.png]]
- **Использовать слабые ссылки (WeakReference)** для кешей или других структур, где объекты могут быть удалены GC, если они больше не используются.
- **Следить за жизненным циклом объектов** и избегать хранения лишних ссылок на них.

-----

### **2. Soft Reference**

<mark class="hltr-yellow">Объекты, на которые ссылаются</mark> <mark class="hltr-red">безопасные ссылки</mark>, <mark class="hltr-green2">удаляются, только если JVM не хватает памяти, т.е. они могут пережить более одной сборки мусора.</mark>
Данный тип ссылок<mark class="hltr-purple"> подходит для реализации кэша - такой структуры данных, при которой часть данных запоминается, а потом часто переиспользуется.</mark>
Например, можно запоминать данные из файлов или тяжелых запросов.
<mark class="hltr-yellow">При нехватке памяти JVM может удалить объекты по этим ссылкам, если на них нет сильных ссылок.</mark>

<mark class="hltr-red">Есть контракт для данного типа ссылок</mark>:<mark class="hltr-green2"> GC гарантировано удалит из кучи все объекты, доступные только по soft-ссылке, перед тем, как бросит OutOfMemoryError</mark>
```java
package ru.job4j.gc.ref;

import java.lang.ref.SoftReference;
import java.util.ArrayList;
import java.util.List;

public class SoftDemo {

    public static void main(String[] args) {
        /* example1(); */
        example2();
    }

    private static void example1() {
        Object strong = new Object();
        SoftReference<Object> soft = new SoftReference<>(strong);
        strong = null;
        System.out.println(soft.get());
    }

    private static void example2() {
        List<SoftReference<Object>> objects = new ArrayList<>();
        for (int i = 0; i < 100_000_000; i++) {
            objects.add(new SoftReference<Object>(new Object() {
                String value = String.valueOf(System.currentTimeMillis());

                @Override
                protected void finalize() throws Throwable {
                    System.out.println("Object removed!");
                }
            }));
        }
        System.gc();
        int liveObject = 0;
        for (SoftReference<Object> ref : objects) {
            Object object = ref.get();
            if (object != null) {
                liveObject++;
            }
        }
        System.out.println(liveObject);
    }

}
```

<mark class="hltr-red">В первом методе, несмотря на то, что мы за'null'уляем сильную ссылку, на объект остается безопасная ссылка, а это значит, что объект будет удален только при нехватке памяти.</mark>

<mark class="hltr-orange">Во втором методе мы создаем много объектов, но на них ссылается безопасная ссылка. При создании большого количества объектов при малом heap мы увидим, что объекты начнут удаляться, т.к. перестанет хватать памяти.  </mark>

<mark class="hltr-purple">Корректным использованием безопасных ссылок является сначала получение сильной ссылки на данные, а потом работа с сильной ссылкой.</mark>

<mark class="hltr-green2">Это гарантирует, что в интервалах получения сильной ссылки из безопасной GC не затрет объект. Это касается не только локальных переменных, но и возвращаемых значений и аргументов.</mark>

Пример,
```java
private static void unsafe() {
    List<SoftReference<Object>> someData = new ArrayList<>();
    if (someData.get(0).get() != null) {
        /* do something */
    } else {
        /* do something */
    }
    /* do something */
    someData.get(0).get();
}

private static void safe() {
    List<SoftReference<Object>> someData = new ArrayList<>();
    Object strong = someData.get(0).get();
    if (strong != null) {
        /* do something */
    } else {
        /* do something */
    }
    /* work with strong */
}
```


-----
### **3. Weak Reference**

Объекты, на которые ссылаются<mark class="hltr-red"> слабые ссылки</mark>, <mark class="hltr-green2">удаляются сразу, если на них нет сильных или безопасных ссылок.</mark>

Данный тип ссылок с<mark class="hltr-yellow">лужит для реализации структур, для которых у одного значения типа может быть только один объект, например пул строк, и объекты чаще всего используется всего один раз, т.е. сохранили-получили-забыли.</mark>

Пример,
```java
package ru.job4j.gc.ref;

import java.lang.ref.WeakReference;
import java.sql.Time;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class WeakDemo {

    public static void main(String[] args) throws InterruptedException {
        /* example1(); */
        example2();
    }

    private static void example1() throws InterruptedException {
        Object object = new Object() {
            @Override
            protected void finalize() throws Throwable {
                System.out.println("Removed");
            }
        };
        WeakReference<Object> weak = new WeakReference<>(object);
        object = null;
        System.gc();
        TimeUnit.SECONDS.sleep(3);
        System.out.println(weak.get());
    }

    private static void example2() throws InterruptedException {
        List<WeakReference<Object>> objects = new ArrayList<>();
        for (int i = 0; i < 100; i++) {
            objects.add(new WeakReference<Object>(new Object() {
                @Override
                protected void finalize() throws Throwable {
                    System.out.println("Removed!");
                }
            }));
        }
        System.gc();
        TimeUnit.SECONDS.sleep(3);
    }

}
```

<mark class="hltr-red">В первом методе за'null'ение сильной ссылки приводит к удалению объекта и мы его также уже не можем получить по слабой ссылке.</mark>
<mark class="hltr-orange">Во втором методе мы создаем объекты вообще без сильных ссылок. При вызове сборщика мусора они все удаляются.</mark>
Корректное использование этого типа ссылок аналогично безопасным.


-----

### **ReferenceQueue**

<mark class="hltr-yellow">Все типы ссылок, за исключением сильных, в Java являются наследниками класса Reference</mark>. <mark class="hltr-red">Все его наследники всегда попадают в ReferenceQueue</mark>, <mark class="hltr-green2">это может происходить явно (мы можем задать свою очередь) или неявно (когда мы не задаем). В нее попадают ссылки тех объектов, которые уже помечены на удаление.</mark>

Особенности ссылок WeakReference и PhantomReference (будет рассмотрена позже) связаны с применением очереди ссылок.

<mark class="hltr-orange">Что касается особенности WeakReference, так это то, что, когда на объект уже нет сильных или безопасных ссылок, происходит за'null'ение слабой ссылки, как в примере выше.</mark>

<mark class="hltr-purple">Далее WeakReference будет помещена в очередь ReferenceQueue и мы можем, пока объект не удален физически, получить его из этой очереди.</mark>

## 🔹 **Что такое `ReferenceQueue`?**

`ReferenceQueue<T>` — это **очередь ссылок**, в которую попадают **объекты ссылочного типа (`WeakReference`, `PhantomReference`)**, когда сборщик мусора решил удалить их оригинальный объект.

📌 **Важно:** В `ReferenceQueue` попадает **сама слабая ссылка**, а не сам объект.

### 🔍 Как это работает?

1️⃣ Если объект больше **не имеет сильных ссылок**, GC решает удалить его.  
2️⃣ Если на объект есть **только `WeakReference`**, GC:

- Обнуляет слабую ссылку (`null`).
- Кладёт её в `ReferenceQueue`.  
    3️⃣ Можно извлечь ссылку из `ReferenceQueue` и выполнить дополнительные действия, пока объект ещё есть физички.

```java
private static void example3() throws InterruptedException {
    Object object = new Object() {
        @Override
        protected void finalize() throws Throwable {
            System.out.println("Removed");
        }
    };
    ReferenceQueue<Object> queue = new ReferenceQueue<>();
    WeakReference<Object> weak = new WeakReference<>(object, queue);
    object = null;

    System.gc();

    TimeUnit.SECONDS.sleep(3);
    System.out.println("from link " + weak.get());
    System.out.println("from queue " + queue.poll());
}
```

![[Pasted image 20250204173040.png]]


```java
import java.lang.ref.ReferenceQueue;
import java.lang.ref.WeakReference;

class Data {
    String value;
    Data(String value) {
        this.value = value;
    }
    @Override
    protected void finalize() {
        System.out.println("💀 Объект " + value + " удалён");
    }
}

public class WeakReferenceExample {
    public static void main(String[] args) throws InterruptedException {
        ReferenceQueue<Data> queue = new ReferenceQueue<>();
        
        Data data = new Data("Привет, мир! 🌍");
        WeakReference<Data> weakRef = new WeakReference<>(data, queue);

        System.out.println("📌 До удаления: " + weakRef.get()); // Выведет объект
        
        data = null; // Убираем сильную ссылку
        System.gc(); // Вызываем сборщик мусора

        Thread.sleep(1000); // Даем GC время

        System.out.println("📌 После GC: " + weakRef.get()); // null, объект удалён
        
        WeakReference<?> refFromQueue = (WeakReference<?>) queue.poll();
        if (refFromQueue != null) {
            System.out.println("🔄 Слабая ссылка попала в ReferenceQueue!");
        }
    }
}

```

### 🔥 **Что здесь происходит?**

1️⃣ Создаём объект `Data` и слабую ссылку на него (`WeakReference`).  
2️⃣ Убираем сильную ссылку (`data = null`).  
3️⃣ Вызываем `System.gc()` для запуска сборщика мусора.  
4️⃣ GC удаляет объект `Data`, потому что на него больше нет **сильных** ссылок.  
5️⃣ `WeakReference` становится `null`.  
6️⃣ **Сама слабая ссылка (`WeakReference`) попадает в `ReferenceQueue`**.

---

## 🔹 **Зачем использовать `WeakReference`?**

✅ **Кеширование данных** — чтобы объекты могли автоматически удаляться при нехватке памяти.  
✅ **Избегание утечек памяти** — когда объект больше не нужен, но его ссылки не удалены.  
✅ **Мониторинг объектов** — можно отслеживать их удаление через `ReferenceQueue`.

-----

### **4. PhantomReference**

<mark class="hltr-red">Фантомный тип ссылок имеет две особенности:</mark>

1) Метод<mark class="hltr-orange"> get() всегда возвращает null, поэтому доступ можно осуществить только через ReferenceQueue</mark>

2) PhantomReference<mark class="hltr-purple"> попадает в ReferenceQueue только после выполнения finalize(), что значит мы еще имеем доступ к объекту некоторое время.</mark>

Данный тип ссылки <mark class="hltr-yellow">используется для более гибкого управления удалением объектов, минуя минусы finalize()</mark> (будут описаны в статье по первой ссылки)

Проведем такой эксперимент:

```java
package ru.job4j.gc.ref;

import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.ListIterator;
import java.util.concurrent.TimeUnit;

public class PhantomDemo {

    private static class MyPhantom extends PhantomReference<String> {

        private String name;

        public MyPhantom(String referent, ReferenceQueue<? super String> q, String name) {
            super(referent, q);
            this.name = name;
        }

        @Override
        public String get() {
            return name;
        }
    }

    private static class PhantomStorage {

        private ReferenceQueue<String> queue = new ReferenceQueue<>();

        private List<MyPhantom> phantoms = new LinkedList<>();

        public void add(String someData) {
            MyPhantom phantom = new MyPhantom(someData, queue, "my ref");
            phantoms.add(phantom);
        }

        public void utilizeResource() {
            for (ListIterator<MyPhantom> i = phantoms.listIterator(); i.hasNext();) {
                MyPhantom current = i.next();
                if (current != null && current.isEnqueued()) {
                    System.out.println("Utilized " + current.get());
                    current.clear();
                    i.remove();
                }
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        PhantomStorage storage = new PhantomStorage();
        String data = "123".repeat(1000);
        storage.add(data);
        data = null;
        System.gc();
        TimeUnit.SECONDS.sleep(3);
        storage.utilizeResource();
    }

}
```

Для начала создаем класс, наследующийся от PhantomReference и переопределяем get(), чтобы проконтролировать, что удаляется наш ресурс. 

Создаем хранилище. В нем есть очередь, которая необходима для ссылок.  Но эта очередь read-only, поэтому создаем свой список и в него помещаем наши фантомные ссылки. Когда вызывается метод для утилизации ресурсов, мы проверяем, есть ли ссылка в очереди, т.е. помечен ли объект на удаление. Далее вызываем явно метод clear(), чтобы указать GC, что нужно удалить объект в будущем и удаляем из нашего списка.


------
## 🔹 SoftReference 🌥️

### Основная идея:

- **SoftReference** позволяет хранить объекты, которые будут оставаться в памяти до тех пор, пока системе хватает ресурсов. Если память заканчивается, GC удаляет такие объекты.

### Примеры использования:

- **Кэш изображений или данных** 📸💾  
    _Пример:_ В приложениях, где нужно временно сохранять данные или изображения. Если их можно легко восстановить или пересоздать, они хранятся в soft-ссылках. При нехватке памяти GC очистит кэш, освобождая ресурсы.
- **Memory-sensitive кеши** ⚙️💡  
    _Плюс:_ Это помогает избежать OutOfMemoryError, так как объекты удаляются автоматически при нехватке памяти.

---

## 🔹 WeakReference 🕸️

### Основная идея:

- **WeakReference** используется для хранения объектов, которые должны быть удалены, как только на них не останется **сильных ссылок**.

### Примеры использования:

- **WeakHashMap** 📚🗑️  
    _Пример:_ В `WeakHashMap` ключи представляются weak-ссылками. Если на ключ больше нет сильных ссылок, соответствующая запись автоматически удаляется, предотвращая утечки памяти.
- **Паттерн "наблюдатель" (Observer)** 👀🔔  
    _Пример:_ При хранении слушателей событий. Если слушатель больше не нужен, weak-ссылка позволяет GC удалить его, не требуя явного удаления.
- **Кеширование с немедленным освобождением** ⏳💨  
    _Плюс:_ Объекты удаляются сразу, как только они больше не используются, что позволяет эффективно управлять памятью.

---

## 🔹 PhantomReference 👻

### Основная идея:

- **PhantomReference** – самый «тонкий» тип ссылки. Метод `get()` всегда возвращает `null`, и они используются исключительно для **отслеживания момента удаления объекта**.

### Примеры использования:

- **Управление нативными ресурсами** 💻🔧  
    _Пример:_ Когда объект связан с нативными ресурсами (например, прямыми буферами, файлами или соединениями с базой данных). Фантомные ссылки позволяют запускать дополнительные операции по освобождению этих ресурсов после того, как объект был финализирован GC.
- **Надёжная очистка без finalize()** 🧹🚀  
    _Пример:_ Вместо метода `finalize()`, который имеет свои недостатки, можно использовать PhantomReference в сочетании с `ReferenceQueue` для гарантированного освобождения ресурсов сразу после того, как объект стал недоступным.
- **Cleaner API в Java 9+** 🧼✨  
    _Пример:_ Новый API использует похожий механизм для управления очисткой ресурсов, делая процесс более надёжным и предсказуемым.

### Как это работает:

1. **Создание фантомной ссылки:**  
    Фантомная ссылка создаётся с привязкой к `ReferenceQueue` 🎯.
2. **Отслеживание удаления объекта:**  
    Когда GC решает, что объект больше не нужен, соответствующая PhantomReference помещается в очередь 🕒.
3. **Очистка ресурсов:**  
    После получения PhantomReference из очереди можно безопасно освободить нативные ресурсы, зная, что объект уже завершил своё "жизненное путешествие" 🌌.

---

## 🔹 Итоговый обзор кейсов 🚀

- **SoftReference** 🌥️  
    _Кейс:_ Кэширование больших объектов (например, изображений) в приложениях, где данные могут быть восстановлены, если память заканчивается.
    
- **WeakReference** 🕸️  
    _Кейс:_ Реализация `WeakHashMap` для автоматического удаления неиспользуемых ключей или хранение слушателей событий в Observer-паттерне для предотвращения утечек памяти.
    
- **PhantomReference** 👻  
    _Кейс:_ Надёжное освобождение нативных ресурсов и выполнение завершающих действий после того, как объект стал недоступен, что особенно полезно в системах с ограниченными ресурсами.


----

📌 **Где хранится ссылка на `i`?**

- Если `i` — **экземплярное поле** (`private Integer i;`), то **ссылка хранится в куче внутри объекта**.
- Если `i` — **локальная переменная** (`Integer i = 10;` внутри метода), то **ссылка хранится в стеке**.
- Если `i` — **`static` поле**, то **ссылка хранится в MetaSpace (или PermGen в старых версиях Java)**, а объект всё равно создаётся в куче
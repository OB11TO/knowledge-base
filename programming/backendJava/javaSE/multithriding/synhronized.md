---
title: synhronized
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:30
modified: 2024-09-11T12:33:50+03:00
questions: 
notes: 
links: 
---
### synhronized

`Synchronized` (синхронизированный) - это ключевое слово в Java, которое используется для обеспечения потокобезопасности и синхронизации доступа к общим ресурсам (переменным, методам, объектам) из нескольких потоков. Оно гарантирует, что только один поток может выполнять код, помеченный как синхронизированный, в любой момент времени.

Основные концепции и способы использования `synchronized`:

1. **Синхронизация методов:** Вы можете сделать целый метод синхронизированным, добавив ключевое слово `synchronized` к его объявлению. Это означает, что только один поток может одновременно выполнять этот метод на данном объекте.
    
    ```Java
    public synchronized void synchronizedMethod() {
        // Код, требующий синхронизации
    }
    ```
    
2. **Синхронизация блоков кода:** Вы также можете синхронизировать конкретные блоки кода, используя ключевое слово `synchronized` и объект блокировки. Это более гибкий способ, который позволяет управлять областью синхронизации.
    
    ```Java
    synchronized (lockObject) {
        // Код, требующий синхронизации
    }
    ```
    
3. **Статическая синхронизация:** В случае, когда нужно синхронизировать статические методы или обращаться к статическим полям класса, вы можете использовать ключевое слово `synchronized` с `static`. Это синхронизирует доступ ко всему классу, а не к конкретному экземпляру.
    
    ```Java
    public static synchronized void synchronizedStaticMethod() {
        // Код, требующий синхронизации для статического метода
    }
    ```
    
    ```Java
    class MyClass
    {
    private static String name1 = "Оля";
    private static String name2 = "Лена";
    
    public synchronized void swap()
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    
    public static synchronized void swap2()
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    }
    
    
    //Что происходит на самом деле
    class MyClass
    {
    private static String name1 = "Оля";
    private static String name2 = "Лена";
    
    public void swap()
    {
    synchronized (this)
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    }
    
    public static void swap2()
    {
    synchronized (MyClass.class)
    {
    String s = name1;
    name1 = name2;
    name2 = s;
    }
    }
    }
    ```
    

Использование `synchronized` позволяет избегать состояний гонки, когда несколько потоков пытаются изменить общие данные одновременно, что может привести к непредсказуемым и нежелательным результатам. Однако следует помнить, что неправильное использование синхронизации может привести к проблемам, таким как блокировки и замедление производительности. Поэтому важно заботиться о правильном проектировании и использовании синхронизации в многопоточных приложениях.

![[images/Untitled 6 3.png|Untitled 6 3.png]]

![[images/Untitled 7 3.png|Untitled 7 3.png]]

![[images/Untitled 8 3.png|Untitled 8 3.png]]

![[images/Untitled 9 3.png|Untitled 9 3.png]]

![[images/Untitled 10 2.png|Untitled 10 2.png]]

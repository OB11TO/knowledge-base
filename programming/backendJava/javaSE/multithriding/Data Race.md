---
title: Data Race, synchronized, volatile, Atomics из java.util.concurrent.atomic
tags:
  - Multithreading
related_topics: 
created: 2024-09-11 12:33
modified: 2024-09-11T12:33:22+03:00
questions: 
notes: 
links: 
---
### Data Race

Проблема: Гонка за данными (Data Race)

```Java
public class Counter {
    private int count = 0;

    public void increment() {
        count++; // Неатомарная операция инкремента
    }

    public int getCount() {
        return count;
    }
}
```

В этом примере `Counter` представляет собой простой счетчик, который не использует атомарные операции для инкремента. Проблема здесь заключается в том, что если несколько потоков вызывают `increment()` одновременно, то может произойти гонка за данными, и значение `count` может быть неверным.
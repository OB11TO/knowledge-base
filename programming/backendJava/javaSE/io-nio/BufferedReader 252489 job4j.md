---
title: BufferedReader 252489 job4j
tags:
  - IO-NIO
  - Job4j
related_topics: 
created: 2025-01-20 10:26
modified: 2025-01-20T10:32:43+03:00
questions: 
notes: 
links: 
---

В предыдущих уроках мы научились читать файл.

```java
        try (FileInputStream input = new FileInputStream("data/input.txt")) {
            StringBuilder text = new StringBuilder();
            int read;
            while ((read = input.read()) != -1) {
                text.append((char) read);
            }
            System.out.println(text);
        } catch (IOException e) {
            e.printStackTrace();
        }
```

<mark class="hltr-yellow">Этот код читает файл по одному байту.</mark> <mark class="hltr-red">Такой подход медленный</mark>. Представьте, что вам нужно перенести 100 тетрадей из одной комнаты в другую. По аналогии с нашим кодом мы стали бы переносить по одной тетради за раз<mark class="hltr-green2">. Другой подход - это взять несколько тетрадей. Так будет быстрее. В потоках ввода вывода для этого есть _буферизированные обертки_ - потоки, имеющие буфер.</mark>

Каждое обращение к внешнему ресурсу - дорогостоящая операция, поэтому буфер помогает уменьшить количество таких обращений.

```java
package ru.job4j.io;

import java.io.*;
import java.util.ArrayList;
import java.util.List;

public class ReadFile {
    public static void main(String[] args) {
        try (BufferedReader input = new BufferedReader(new FileReader("data/input.txt"))) {
            input.lines().forEach(System.out::println);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Аналог
```java
for (String line = input.readLine(); line != null; line = input.readLine()) {
     System.out.println(line);
}
```


-![[Pasted image 20250120103000.png]]


![[Pasted image 20250120103232.png]]


---
title: Потоки данных (Data Streams) 504989 job4j
tags:
  - IO-NIO
  - Job4j
related_topics: 
created: 2025-01-21 17:13
modified: 2025-01-21T17:21:53+03:00
questions: 
notes: 
links: 
---


В этом уроке мы познакомимся с классами DataInputStream и DataOutputStream.

<mark class="hltr-yellow">Класс DataOutputStream позволяет записывать примитивные типы данных, а также строковые значения в байтовом представлении, а класс DataInputStream позволяет считывать примитивы из их байтового представления. 
</mark>

Классы DataInputStream и DataOutputStream _реализуют_ интерфейсы DataInput и DataOutput соответственно. Эти интерфейсы содержат методы, с помощью которых можно читать и записывать примитивы и строки в поток. Например, writeBoolean(), writeDouble(), readBoolean(), readDouble() и т.д. Эти методы преобразуют значения примитивных типов в последовательность байтов и наоборот. Таким образом упрощается хранение данных в двоичной форме в файлах.

![[Pasted image 20250121171625.png]]

```java
package ru.job4j.io;

import java.io.*;

public class DataStream {
    public static void main(String[] args) throws Exception {
        String path = "data/dataOutput.txt";
        String[] names = {"unit1", "unit2", "unit3"};
        int[] amounts = {5, 7, 2};
        boolean[] checked = {true, false, true};

        try (DataOutputStream output = new DataOutputStream(new FileOutputStream(path));
             DataInputStream input = new DataInputStream(new FileInputStream(path))) {
            for (int i = 0; i < names.length; i++) {
                output.writeUTF(names[i]);
                output.writeInt(amounts[i]);
                output.writeBoolean(checked[i]);
            }
            while (true) {
                String name = input.readUTF();
                int amount = input.readInt();
                boolean check = input.readBoolean();
                System.out.println("Наименование: " + name 
                        + ", количество: " + amount 
                        + ", проверен: " + check);
            }
        } catch (EOFException e) {
            System.out.println("Достигнут конец файла");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```


![[Pasted image 20250121171909.png]]


![[Pasted image 20250121172155.png]]
---
title: DataOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:55
modified: 2024-09-10T18:56:13+03:00
questions: 
notes: 
links: 
---
### 5. **`DataOutputStream`**

- **Описание**: <mark class="hltr-purple">Поток вывода для записи примитивных типов данных</mark> (int, float, long и т.д.).
- **Особенности**:
    - Позволяет<mark class="hltr-yellow"> записывать примитивные типы данных</mark> (например, числа и строки) в <mark class="hltr-yellow">байтовый поток в формате, подходящем для последующего чтения с использованием</mark> `DataInputStream`.
    - Полезен при работе с <mark class="hltr-red">бинарными форматами</mark> данных.
    - Он предоставляет методы, такие как `**writeInt()**`, `**writeDouble()**`, `**writeUTF()**`, которые позволяют записывать данные определенного типа
- **Пример**:
    
```java
  FileOutputStream fos = new FileOutputStream("data.bin");
DataOutputStream dos = new DataOutputStream(fos);
dos.writeInt(123);
dos.writeDouble(10.5);
dos.close();

```

```java
    import java.io.DataOutputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    
    public class DataOutputStreamExample {
        public static void main(String[] args) {
            try {
                FileOutputStream fos = new FileOutputStream("data.bin");
                DataOutputStream dos = new DataOutputStream(fos);
    
                // Записываем данные в поток вывода
                dos.writeInt(42);
                dos.writeDouble(3.14);
                dos.writeUTF("Hello");
    
                // Закрываем потоки
                dos.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
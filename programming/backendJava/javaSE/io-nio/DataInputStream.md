---
title: DataInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:50
modified: 2024-09-10T18:50:37+03:00
questions: 
notes: 
links: 
---
### 5. **`DataInputStream`**

- **Описание**: Используется <mark class="hltr-purple">для чтения примитивных типов данных</mark> (int, long, float и т.д.) из байтового потока.
- **Особенности**:
    - Читает <mark class="hltr-yellow">данные в формате, подходящем для использования примитивных типов </mark>(например, `int`, `double`).
    - <mark class="hltr-green2">Полезен для работы с потоками, которые содержат бинарные данные.</mark>
- **Пример**:
    
```java
FileInputStream fis = new FileInputStream("data.bin");
DataInputStream dis = new DataInputStream(fis);
int number = dis.readInt();
System.out.println(number);
dis.close();


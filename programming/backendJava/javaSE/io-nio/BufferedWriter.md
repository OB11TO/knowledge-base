---
title: BufferedWriter
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:23
modified: 2024-09-11T11:23:26+03:00
questions: 
notes: 
links: 
---
### 3. **`BufferedWriter`**

- **Описание**: Поток вывода с <mark class="hltr-purple">буферизацией</mark> для повышения производительности.
- **Особенности**:
    - Оборачивает другой поток вывода (например, `FileWriter`), добавляя буфер для уменьшения количества операций записи.
    - Поддерживает запись строк и предоставляет метод `newLine()` для вставки символов новой строки.
- **Пример**:
    
```java
BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"));
bw.write("Buffered data");
bw.newLine();  // Добавляет новую строку
bw.close();

```

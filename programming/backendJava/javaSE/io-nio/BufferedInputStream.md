---
title: BufferedInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:50
modified: 2025-01-14T12:54:38+03:00
questions: 
notes: 
links: 
---

### 4. **`BufferedInputStream`**

- **Описание**: Предоставляет <mark class="hltr-purple">возможность буферизованного ввода для повышения производительности</mark> при чтении байтов из потока.
- **Особенности**:
    - <mark class="hltr-yellow">Оборачивает другой поток ввода</mark> (например, `FileInputStream`), добавляя буфер для повышения эффективности.
    - <mark class="hltr-yellow">Уменьшает количество операций чтения с диска или сети</mark>.
- **Пример**:
    
```java
FileInputStream fis = new FileInputStream("file.txt");
BufferedInputStream bis = new BufferedInputStream(fis);
int data = bis.read();
while (data != -1) {
    System.out.print((char) data);
    data = bis.read();
}
bis.close();

```

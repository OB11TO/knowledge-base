---
title: BufferedReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:19
modified: 2025-01-14T12:57:56+03:00
questions: 
notes: 
links: 
---

### 3. **`BufferedReader`**

- **Описание**: Добавляет <mark class="hltr-purple">буферизацию</mark> к потоку ввода символов для повышения производительности.
- **Особенности**:
    - Оборачивает другой поток ввода (например, `FileReader`), добавляя буфер для уменьшения количества операций ввода.
    - <mark class="hltr-yellow">Поддерживает чтение строк и предоставляет метод</mark> `readLine()` для удобного чтения строк.
- **Пример**:
    
```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
String line;
while ((line = br.readLine()) != null) {
    System.out.println(line);
}
br.close();

```

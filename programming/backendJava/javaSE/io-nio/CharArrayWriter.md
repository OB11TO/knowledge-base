---
title: CharArrayWriter
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:23
modified: 2024-09-11T11:23:40+03:00
questions: 
notes: 
links: 
---
### 4. **`CharArrayWriter`**

- **Описание**: <mark class="hltr-purple">Записывает символы в массив символов, который находится в памяти.</mark>
- **Особенности**:
    - Полезен для работы с временными данными в памяти.
    - Позволяет получить массив символов через метод `toCharArray()`.
- **Пример**:
    
```java
CharArrayWriter caw = new CharArrayWriter();
caw.write("Hello".toCharArray());
char[] data = caw.toCharArray();
caw.close();

```


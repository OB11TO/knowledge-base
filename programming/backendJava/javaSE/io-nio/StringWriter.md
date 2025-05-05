---
title: StringWriter
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:23
modified: 2024-09-11T11:23:51+03:00
questions: 
notes: 
links: 
---
### 5. **`StringWriter`**

- **Описание**: <mark class="hltr-yellow">Записывает символы в строку.</mark>
- **Особенности**:
    - Полезен, когда нужно накопить текст в строку для последующей обработки.
    - Позволяет получить строку через метод `toString()`.
- **Пример**:
    
```java
StringWriter sw = new StringWriter();
sw.write("Hello, World!");
String result = sw.toString();
sw.close();


---
title: PrintWriter
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:23
modified: 2024-09-11T11:24:07+03:00
questions: 
notes: 
links: 
---
### 6. **`PrintWriter`**

- **Описание**: Поток вывода, <mark class="hltr-red">который добавляет методы для удобной записи строк и других данных </mark>(например, `print()`, `println()`).
- **Особенности**:
    - Обеспечивает удобный синтаксис для записи данных <mark class="hltr-pink">и автоматическое управление ошибками </mark>(подавляет `IOException`).
- **Пример**:
    
```java
PrintWriter pw = new PrintWriter(new FileWriter("output.txt"));
pw.println("Hello, World!");
pw.close();

```

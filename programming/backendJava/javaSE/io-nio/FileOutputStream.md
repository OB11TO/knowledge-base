---
title: FileOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:56
modified: 2025-01-14T12:56:56+03:00
questions: 
notes: 
links: 
---

### 2. **`FileOutputStream`**

- **Описание**: Поток <mark class="hltr-purple">вывода для записи байтов в файл.</mark>
- **Особенности**:
    - Используется <mark class="hltr-yellow">для записи данных в файл на файловой системе.</mark>
    - Можно указать, дописывать ли данные в файл или перезаписывать его (через соответствующий конструктор).
- **Пример**:
    
```java
FileOutputStream fos = new FileOutputStream("output.txt");
fos.write("Hello, World!".getBytes());
fos.close();


---
title: PipedInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:51
modified: 2024-09-10T18:51:34+03:00
questions: 
notes: 
links: 
---
### 8. **`PipedInputStream`**

- **Описание**: Используется<mark class="hltr-red"> для связи между потоками. Читает данные из канала (pipe), которые были записаны в другой поток с помощью</mark> `PipedOutputStream`.
- **Особенности**:
    - Позволяет<mark class="hltr-purple"> организовать межпотоковое взаимодействие через каналы.</mark>
    - Используется в многопоточных приложениях для передачи данных между потоками.
- **Пример**:
    
```java
PipedInputStream pis = new PipedInputStream();
PipedOutputStream pos = new PipedOutputStream(pis);

```

    
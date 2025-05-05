---
title: PipedWriter
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:24
modified: 2024-09-11T11:24:29+03:00
questions: 
notes: 
links: 
---
### 8. **`PipedWriter`**

- **Описание**: Поток вывода для <mark class="hltr-yellow">межпотоковой</mark> передачи данных, записывающий данные, которые могут быть прочитаны через `PipedReader`.
- **Особенности**:
    - Используется для организации связи между потоками через каналы (pipe).
- **Пример**:
    
```java
PipedWriter pw = new PipedWriter();
PipedReader pr = new PipedReader(pw);

```

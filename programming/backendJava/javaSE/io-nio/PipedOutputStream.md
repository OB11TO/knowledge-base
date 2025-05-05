---
title: PipedOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:55
modified: 2024-09-10T18:55:23+03:00
questions: 
notes: 
links: 
---
### 8. **`PipedOutputStream`**

- **Описание**: Поток вывода для <mark class="hltr-purple">межпотоковой передачи данных</mark>. Записывает данные, которые могут быть прочитаны через `PipedInputStream`.
- **Особенности**:
    - Используется для организации связи между потоками <mark class="hltr-yellow">через каналы (pipe).</mark>
    - Полезен в многопоточных приложениях, где один поток записывает данные, а другой их читает.
- **Пример**:
    
```java
 PipedOutputStream pos = new PipedOutputStream();
PipedInputStream pis = new PipedInputStream(pos);

```

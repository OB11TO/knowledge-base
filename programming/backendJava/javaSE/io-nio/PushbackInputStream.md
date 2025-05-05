---
title: PushbackInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:51
modified: 2024-09-10T18:52:03+03:00
questions: 
notes: 
links: 
---

### 10. **`PushbackInputStream`**

- **Описание**: Предоставляет <mark class="hltr-yellow">возможность</mark> "<mark class="hltr-red">откатывания</mark>" пр<mark class="hltr-yellow">очитанных байтов обратно в поток, чтобы они могли быть прочитаны снова.</mark>
- **Особенности**:
    - Полезен в случаях, когда нужно «вернуть» байты обратно в поток для повторного чтения.
- **Пример**:
    
```java
PushbackInputStream pis = new PushbackInputStream(new FileInputStream("file.txt"));
int data = pis.read();
pis.unread(data);  // Возвращаем байт обратно в поток

```


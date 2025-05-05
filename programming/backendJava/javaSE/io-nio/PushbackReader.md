---
title: PushbackReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:20
modified: 2024-09-11T11:20:26+03:00
questions: 
notes: 
links: 
---
### 6. **`PushbackReader`**

- **Описание**: Позволяет <mark class="hltr-red">возвращать прочитанные символы обратно в поток для повторного чтения.</mark>
- **Особенности**:
    - Полезен для парсинга текстов, где может потребоваться возвращение символов обратно в поток.
- **Пример**:
    
```java
PushbackReader pr = new PushbackReader(new FileReader("file.txt"));
int charData = pr.read();
pr.unread(charData);  // Возвращаем символ обратно в поток
pr.close();


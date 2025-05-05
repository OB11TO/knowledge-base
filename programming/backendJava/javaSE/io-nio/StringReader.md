---
title: StringReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:19
modified: 2024-09-11T11:20:12+03:00
questions: 
notes: 
links: 
---
### 5. **`StringReader`**

- **Описание**: <mark class="hltr-purple">Читает символы из строки.</mark>
- **Особенности**:
    - Полезен, когда нужно обрабатывать строку как поток символов.
- **Пример**:
    
```java
StringReader sr = new StringReader("Hello, World!");
int charData;
while ((charData = sr.read()) != -1) {
    System.out.print((char) charData);
}
sr.close();

```

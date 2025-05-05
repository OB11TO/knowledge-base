---
title: SequenceOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:54
modified: 2024-09-10T18:54:49+03:00
questions: 
notes: 
links: 
---
### 10. **`SequenceOutputStream`**

- **Описание**: <mark class="hltr-yellow">Объединяет несколько потоков вывода в один</mark>. Записывает данные последовательно в несколько потоков.
- **Особенности**:
    - Полезен, если нужно объединить <mark class="hltr-green2">несколько источников данных для записи в один поток вывода.</mark>
- **Пример**:
    
```java
FileOutputStream fos1 = new FileOutputStream("file1.txt");
FileOutputStream fos2 = new FileOutputStream("file2.txt");
SequenceOutputStream sos = new SequenceOutputStream(fos1, fos2);

```


---


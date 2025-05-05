---
title: SequenceInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:51
modified: 2024-09-10T18:51:49+03:00
questions: 
notes: 
links: 
---
### 9. **`SequenceInputStream`**

- **Описание**: <mark class="hltr-yellow">Объединяет несколько потоков ввода в один</mark>. <mark class="hltr-green2">Читает данные из одного потока за другим последовательно.</mark>
- **Особенности**:
    - Полезен для последовательного <mark class="hltr-purple">чтения данных из нескольких потоков</mark> (например, из нескольких файлов).
- **Пример**:
    
```java
FileInputStream fis1 = new FileInputStream("file1.txt");
FileInputStream fis2 = new FileInputStream("file2.txt");
SequenceInputStream sis = new SequenceInputStream(fis1, fis2);


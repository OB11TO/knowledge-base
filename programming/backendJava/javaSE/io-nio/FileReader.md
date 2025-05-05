---
title: FileReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:19
modified: 2024-09-11T11:19:23+03:00
questions: 
notes: 
links: 
---
### 2. **`FileReader`**

- **Описание**: Используется для <mark class="hltr-purple">чтения символов из файлов.</mark>
- **Особенности**:
    - <mark class="hltr-yellow">Читает текстовые файлы как последовательность символов.</mark>
    - Подходит для работы<mark class="hltr-yellow"> с текстовыми данными</mark>, сохраненными в файлах.
- **Пример**:
    
```java
FileReader fr = new FileReader("file.txt");
int data = fr.read();
while (data != -1) {
    System.out.print((char) data);
    data = fr.read();
}
fr.close();


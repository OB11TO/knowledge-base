---
title: ObjectOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:55
modified: 2024-09-10T18:55:41+03:00
questions: 
notes: 
links: 
---
### 7. **`ObjectOutputStream`**

- **Описание**: Поток вывода <mark class="hltr-red">для сериализации объектов.</mark>
- **Особенности**:
    - Позволяет записывать объекты в поток для их последующего восстановления через `ObjectInputStream` (процесс называется сериализация).
    - Полезен для сохранения состояния объектов в файлы или передачи объектов через сеть.
- **Пример**:
    
```java
   FileOutputStream fos = new FileOutputStream("objectData.ser");
ObjectOutputStream oos = new ObjectOutputStream(fos);
oos.writeObject(new MyObject());
oos.close();


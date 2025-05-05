---
title: ObjectInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:51
modified: 2024-09-10T18:51:20+03:00
questions: 
notes: 
links: 
---
### 7. **`ObjectInputStream`**

- **Описание**: Используется <mark class="hltr-purple">для десериализации объектов из байтового потока, то есть для преобразования байтов обратно в объекты Java.</mark>
- **Особенности**:
    -<mark class="hltr-yellow"> Позволяет читать объекты, которые были сериализованы через</mark> `ObjectOutputStream`.
    - Полезен для работы с сериализацией объектов.
- **Пример**:
```java
FileInputStream fis = new FileInputStream("objectData.ser");
ObjectInputStream ois = new ObjectInputStream(fis);
MyObject obj = (MyObject) ois.readObject();
ois.close();

```

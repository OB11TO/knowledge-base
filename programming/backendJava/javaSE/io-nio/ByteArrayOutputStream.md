---
title: ByteArrayOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:56
modified: 2025-01-14T12:57:04+03:00
questions: 
notes: 
links: 
---

### 3. **`ByteArrayOutputStream`**

- **Описание**: Поток в<mark class="hltr-purple">ывода для записи байтов в массив байтов в памяти</mark>.
- **Особенности**:
    - Данные <mark class="hltr-yellow">записываются в массив байтов, который находится в оперативной памяти, а не на диске.</mark>
    - Полезен для работы с временными данными.
- **Методы**:
    - `toByteArray()`: Возвращает все записанные байты в виде массива байтов.
- **Пример**:

```java
ByteArrayOutputStream baos = new ByteArrayOutputStream();
baos.write("Hello".getBytes());
byte[] data = baos.toByteArray();
baos.close();


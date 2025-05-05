---
title: ByteArrayInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:49
modified: 2024-09-10T18:49:58+03:00
questions: 
notes: 
links: 
---
### 3. **`ByteArrayInputStream`**

- **Описание**: <mark class="hltr-purple">Читает байты из массива байтов</mark> как из потока.
- **Особенности**:
    - Поток <mark class="hltr-yellow">работает с массивом байтов, который содержится в памяти.</mark>
    - <mark class="hltr-yellow">Полезен</mark>, когда нужно о<mark class="hltr-yellow">брабатывать данные, находящиеся в памяти, как поток</mark>.
- **Пример**:

```java
byte[] data = "Hello".getBytes();
ByteArrayInputStream bais = new ByteArrayInputStream(data);
int byteData;
while ((byteData = bais.read()) != -1) {
    System.out.print((char) byteData);
}
bais.close();

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.IOException;

public class InputStreamExample {
    public static void main(String[] args) {
        byte[] bytes = { 65, 66, 67, 68, 69 };
        InputStream inputStream = new ByteArrayInputStream(bytes);

        try {
            int byteValue;
            while ((byteValue = inputStream.read()) != -1) {
                // Обработка прочитанного байта
                System.out.println(byteValue);
            }

            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

---
title: CharArrayReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:19
modified: 2024-09-11T11:19:57+03:00
questions: 
notes: 
links: 
---
### 4. **`CharArrayReader`**

- **Описание**: Читает<mark class="hltr-purple"> символы из массива символов как из потока.</mark>
- **Особенности**:
    - Полезен, когда нужно обрабатывать данные, находящиеся в массиве символов.
- **Пример**:
    
```java
char[] data = "Hello".toCharArray();
CharArrayReader car = new CharArrayReader(data);
int charData;
while ((charData = car.read()) != -1) {
    System.out.print((char) charData);
}
car.close();

import java.io.CharArrayReader;
import java.io.Reader;
import java.io.IOException;

public class CharArrayReaderExample {
    public static void main(String[] args) {
        char[] chars = { 'H', 'e', 'l', 'l', 'o', '!', ' ', 'w', 'o', 'r', 'l', 'd' };
        Reader reader = new CharArrayReader(chars);

        try {
            int charValue;
            while ((charValue = reader.read()) != -1) {
                // Обработка прочитанного символа
                System.out.println((char) charValue);
            }

            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


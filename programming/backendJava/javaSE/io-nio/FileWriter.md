---
title: FileWriter
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:22
modified: 2024-09-11T11:23:12+03:00
questions: 
notes: 
links: 
---
### 2. **`FileWriter`**

- **Описание**: Поток вывода<mark class="hltr-purple"> для записи символов в файл.</mark>
- **Особенности**:
    - Записывает текстовые данные в файл на файловой системе.
    - Можно указать, дописывать ли данные в файл или перезаписывать его.
- **Пример**:
    
```java
FileWriter fw = new FileWriter("output.txt");
fw.write("Hello, World!");
fw.close();

import java.io.FileWriter;
import java.io.IOException;
import java.io.Writer;

public class WriterExample {
    public static void main(String[] args) {
        try {
            // Создаем объект FileWriter для файла "output.txt"
            Writer writer = new FileWriter("output.txt");

            // Записываем символы в поток вывода
            writer.write('H');
            writer.write("ello, ");
            writer.write("World!");

            // Сбрасываем буфер и записываем оставшиеся символы
            writer.flush();

            // Закрываем поток вывода
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

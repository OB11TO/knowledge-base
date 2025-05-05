---
title: PrintStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:54
modified: 2024-09-10T18:55:09+03:00
questions: 
notes: 
links: 
---
### 9. **`PrintStream`**

- **Описание**: <mark class="hltr-red">Поток вывода, который добавляет методы для удобной записи строк, а также автоматически обрабатывает ошибки</mark> (может подавлять `IOException`).
- **Особенности**:
    - Обладает методами `print()` и `println()`, что делает его <mark class="hltr-yellow">удобным для записи текстовых данных.</mark>
    - Может использоваться <mark class="hltr-green2">для работы с консолью, файлами или любыми другими потоками вывода.</mark>
- **Пример**:
    
```java

PrintStream ps = new PrintStream(new FileOutputStream("output.txt"));
ps.println("Hello, World!");
ps.close();

import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.PrintStream;
    
    public class PrintStreamExample {
        public static void main(String[] args) {
            try {
                FileOutputStream fos = new FileOutputStream("output.txt");
                PrintStream ps = new PrintStream(fos);
    
                // Выводим текст в поток вывода
                ps.println("Hello, world!");
                ps.printf("The answer is %d", 42);
    
                // Закрываем потоки
                ps.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

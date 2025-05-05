---
title: InputStreamReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:20
modified: 2024-09-11T11:21:14+03:00
questions: 
notes: 
links: 
---
### 9.  `InputStreamReader` 
преобразует байтовой поток в символьный поток путем чтения байтов и декодирования их в символы с использованием определенной кодировки.)

Пример использования класса `**InputStreamReader**` для чтения текстового файла с определенной кодировкой:

```java
		import java.io.FileInputStream;
        import java.io.InputStreamReader;
        import java.io.BufferedReader;
        import java.io.Reader;
        import java.nio.charset.StandardCharsets;
        import java.io.IOException;
        
        public class InputStreamReaderExample {
            public static void main(String[] args) {
                try {
                    FileInputStream fileInputStream = new FileInputStream("file.txt");
                    Reader reader = new InputStreamReader(fileInputStream, StandardCharsets.UTF_8);
                    BufferedReader bufferedReader = new BufferedReader(reader);
        
                    String line;
                    while ((line = bufferedReader.readLine()) != null) {
                        // Обработка прочитанной строки
                        System.out.println(line);
                    }
        
                    bufferedReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
```

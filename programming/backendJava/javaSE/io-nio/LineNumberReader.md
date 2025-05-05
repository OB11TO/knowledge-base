---
title: LineNumberReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:20
modified: 2024-09-11T11:20:54+03:00
questions: 
notes: 
links: 
---
### 8. **`LineNumberReader`**

- **Описание**: <mark class="hltr-red">Расширяет функциональность</mark> `BufferedReader`, <mark class="hltr-yellow">добавляя возможность отслеживания номеров строк.</mark>
- **Особенности**:
    - Полезен <mark class="hltr-yellow">при чтении текстов, где нужно знать номер строки.</mark>
- **Пример**:
    
```java
LineNumberReader lnr = new LineNumberReader(new FileReader("file.txt"));
String line;
while ((line = lnr.readLine()) != null) {
    System.out.println("Line " + lnr.getLineNumber() + ": " + line);
}
lnr.close();

```

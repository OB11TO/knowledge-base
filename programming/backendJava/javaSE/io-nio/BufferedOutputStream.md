---
title: BufferedOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:56
modified: 2025-01-14T12:57:13+03:00
questions: 
notes: 
links: 
---

### 4. **`BufferedOutputStream`**

[[Как работает BufferedInputStream и BufferedOutputStream в Java]]

- **Описание**: Поток вывода с <mark class="hltr-red">буферизацией</mark> для повышения производительности.
- **Особенности**:
    - <mark class="hltr-green2">Оборачивает другой поток вывода</mark> (например, `FileOutputStream`) и использует буфер для временного хранения данных перед записью на диск.
    - Уменьшает количество операций ввода-вывода, повышая эффективность.
    - Он <mark class="hltr-yellow">улучшает производительность путем минимизации фактических обращений к файловой системе.</mark>
- **Пример**:
    
```java
 FileOutputStream fos = new FileOutputStream("output.txt");
BufferedOutputStream bos = new BufferedOutputStream(fos);
bos.write("Buffered data".getBytes());
bos.flush();  // Принудительная запись данных из буфера
bos.close();

```

```java
  import java.io.BufferedOutputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    
    public class BufferedOutputStreamExample {
        public static void main(String[] args) {
            try {
                FileOutputStream fos = new FileOutputStream("output.txt");
                BufferedOutputStream bos = new BufferedOutputStream(fos);
    
                // Записываем байты в буферизованный поток вывода
                bos.write(65); // Записываем ASCII-код символа 'A'
                bos.write(new byte[]{66, 67, 68}); // Записываем массив байтов
    
                // Сбрасываем буфер и записываем оставшиеся символы в файл
                bos.flush();
    
                // Закрываем потоки
                bos.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

```

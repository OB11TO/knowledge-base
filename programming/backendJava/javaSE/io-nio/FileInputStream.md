---
title: FileInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:49
modified: 2025-01-14T12:53:33+03:00
questions: 
notes: 
links: 
---

### **`FileInputStream`**

- **Описание**: Используется<mark class="hltr-purple"> для чтения байтов из файлов</mark>.
- **Особенности**:
    - <mark class="hltr-yellow">Может читать файл как последовательность байтов.</mark>
    - Поддерживает чтение из существующих файлов на файловой системе.
- **Пример**:
```java
FileInputStream fis = new FileInputStream("file.txt");
int data = fis.read();
while (data != -1) {
    System.out.print((char) data);
    data = fis.read();
}
fis.close();

    public static void main(String[] args) {
        try {
            InputStream inputStream = new FileInputStream("file.txt");
            // читаем данные в буффер
            byte[] buffer = new byte[1024];
            int bytesRead;

            while ((bytesRead = inputStream.read(buffer)) != -1) {
                // Обработка прочитанных байтов
                for (int i = 0; i < bytesRead; i++) {
                    System.out.println(buffer[i]);
                }
            }

            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

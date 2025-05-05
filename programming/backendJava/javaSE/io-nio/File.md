---
title: File
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 17:49
modified: 2025-01-14T13:15:58+03:00
questions: 
notes: 
links: 
---

### File

Класс `File` в пакете `java.io` представляет файл или директорию в файловой системе. Он используется для работы с файлами и папками, включая создание, удаление, перемещение и проверку свойств файлов.

==ЭТОТ КЛАСС СОВМЕСТИМ С NIO==

Некоторые из основных методов класса `File` включают:

- `toPath()`: <mark class="hltr-yellow">Возвращает объект типа</mark> `Path`, представляющий путь к файлу или директории.
- `exists()`: Проверяет, существует ли файл или директория.
- `isDirectory()`: Проверяет, является ли объект `File` директорией.
- `isFile()`: Проверяет, является ли объект `File` обычным файлом.
- `createNewFile()`: Создает новый файл.
- `mkdir()`: Создает директорию.
- `mkdirs()`: Создает директорию, включая все промежуточные директории.
- `delete()`: Удаляет файл или директорию.
- `renameTo(File dest)`: Переименовывает файл или директорию на указанный путь.
- `getParent()`: Возвращает родительскую директорию файла или директории.
- `lastModified()`: Возвращает время последней модификации файла или директории.

Примеры использования класса `File`:

1. Создание нового файла и запись в него:

```Java
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileExample {
    public static void main(String[] args) {
        File file = new File("path/to/file.txt");
        
        try {
            if (file.createNewFile()) {
                System.out.println("Файл создан: " + file.getAbsolutePath());
                
                // Запись в файл
                FileWriter writer = new FileWriter(file);
                writer.write("Привет, мир!");
                writer.close();
            } else {
                System.out.println("Файл уже существует.");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

2. Проверка существования файла и удаление его, если существует:

```Java
import java.io.File;

public class FileExample {
    public static void main(String[] args) {
        File file = new File("path/to/file.txt");
        
        if (file.exists()) {
            if (file.delete()) {
                System.out.println("Файл удален.");
            } else {
                System.out.println("Не удалось удалить файл.");
            }
        } else {
            System.out.println("Файл не существует.");
        }
    }
}

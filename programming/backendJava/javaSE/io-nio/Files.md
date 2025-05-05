---
title: Files
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:19
modified: 2024-09-10T18:24:41+03:00
questions: 
notes: 
links: 
---
Класс `Files` в пакете `java.nio.file` предоставляет мощные и удобные методы для работы с файлами и директориями. Этот класс значительно упрощает операции с файловой системой по сравнению с традиционными методами ввода-вывода.
### Основные методы класса `Files`:
**exists(Path path, LinkOption... options)**: Проверяет, существует ли файл или директория по указанному пути.

```java
Path path = Paths.get("example.txt");
boolean exists = Files.exists(path);

```

**createFile(Path path, FileAttribute\<?>... attrs)**: Создает новый файл по указанному пути.

```java
Path path = Paths.get("newfile.txt");
Files.createFile(path);

```
**createDirectory(Path dir, FileAttribute\<?>... attrs)**: Создает новую директорию по указанному пути.

```java
Path dir = Paths.get("newdirectory");
Files.createDirectory(dir);

```

![[Pasted image 20240910182336.png]]
![[Pasted image 20240910182349.png]]
![[Pasted image 20240910182401.png]]
![[Pasted image 20240910182411.png]]
![[Pasted image 20240910182423.png]]
![[Pasted image 20240910182431.png]]
1. Создание нового файла:

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.io.IOException;

public class FilesExample {
    public static void main(String[] args) {
        Path filePath = Path.of("path/to/file.txt");
        
        try {
            Files.createFile(filePath);
            System.out.println("Файл создан: " + filePath.toAbsolutePath());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

2. Копирование файла:
```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.io.IOException;

public class FilesExample {
    public static void main(String[] args) {
        Path sourcePath = Path.of("path/to/source.txt");
        Path targetPath = Path.of("path/to/target.txt");
        
        try {
            Files.copy(sourcePath, targetPath);
            System.out.println("Файл скопирован.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```


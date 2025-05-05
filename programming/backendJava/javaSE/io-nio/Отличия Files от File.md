---
title: Отличия Files от File
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:24
modified: 2024-09-10T18:29:21+03:00
questions: 
notes: 
links: 
---
Классы `File` и `Files` предоставляют разные подходы и возможности для работы с файлами и директориями в Java. Вот более подробное сравнение:

### Основные отличия между `File` и `Files`:

1. **API-интерфейс**:
    
    - **`File`**: Класс `File` был введен в Java 1.0. Это старый API, который предоставляет методы для работы с файлами и директориями, но имеет некоторые ограничения по функциональности и удобству.
    - **`Files`**: Класс `Files` был представлен в Java 7 как часть NIO (New Input/Output). Это современный API, предоставляющий более удобные и мощные методы для работы с файловой системой.
2. **Объекты представления**:
    
    - **`File`**: Представляет файл или директорию как объект. Вы создаете экземпляры класса `File` для представления файлов или директорий и вызываете методы этого экземпляра для выполнения операций.

```java
File file = new File("example.txt");
file.createNewFile();

```

**`Files`**: Это утилитный класс, который предоставляет статические методы для выполнения операций над файлами и директориями. Он не представляет отдельные файлы или директории как объекты.
```java
Path path = Paths.get("example.txt");
Files.createFile(path);

```

3. **Функциональность**:

- **`File`**: Предоставляет базовые методы для создания, удаления, перемещения, проверки существования файлов и директорий.
```java
File file = new File("example.txt");
boolean exists = file.exists();
file.delete();

```

**`Files`**: Предоставляет более расширенные возможности, такие как:

- Копирование и перемещение файлов и директорий.
- Чтение и запись файлов с использованием потоков и буферов.
- Работа с атрибутами файлов (например, проверка скрытых файлов, получение размеров).
- Создание и удаление директорий, а также создание символических ссылок.
```java
Path source = Paths.get("source.txt");
Path target = Paths.get("target.txt");
Files.copy(source, target);

```

4. **Обработка исключений**:

- **`File`**: Генерирует исключения типа `IOException` для большинства операций. Эти исключения могут быть менее специфичными, что затрудняет диагностику проблем.
```java
try {
    File file = new File("example.txt");
    boolean created = file.createNewFile();
} catch (IOException e) {
    e.printStackTrace();
}

```

**`Files`**: Генерирует более конкретные исключения, такие как `NoSuchFileException`, `FileAlreadyExistsException`, и `DirectoryNotEmptyException`. Это помогает лучше справляться с ошибками.

```java
try {
    Path path = Paths.get("example.txt");
    Files.createFile(path);
} catch (FileAlreadyExistsException e) {
    System.out.println("File already exists.");
} catch (IOException e) {
    e.printStackTrace();
}

```

### Примеры использования:

**Использование `File`:**
```java
import java.io.File;
import java.io.IOException;

public class FileExample {
    public static void main(String[] args) {
        File file = new File("example.txt");
        
        try {
            if (file.createNewFile()) {
                System.out.println("File created: " + file.getName());
            } else {
                System.out.println("File already exists.");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```


Использование `Files`:

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;
import java.io.IOException;

public class FilesExample {
    public static void main(String[] args) {
        Path path = Paths.get("example.txt");
        
        try {
            if (!Files.exists(path)) {
                Files.createFile(path);
                System.out.println("File created: " + path.getFileName());
            } else {
                System.out.println("File already exists.");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```


В общем, класс `Files` представляет более современный и мощный подход к работе с файловой системой в Java и рекомендуется для использования в новых проектах.
---
title: OpenOption
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:41
modified: 2024-09-10T18:42:32+03:00
questions: 
notes: 
links: 
---
### OpenOption

Интерфейс представляет опции для открытия файлов и других операций ввода-вывода. Он определяет методы, которые используются для указания дополнительных параметров при открытии файла.

Реализации:

1. `StandardOpenOption`: Перечисление, предоставляющее стандартные опции открытия файлов. Некоторые из наиболее распространенных значения включают:
    - `READ`: Файл открывается для чтения.
    - `WRITE`: Файл открывается для записи.
    - `APPEND`: Запись в файл будет происходить в конец файла.
    - `CREATE`: Если файл не существует, он будет создан.
    - `TRUNCATE_EXISTING`: Существующий файл будет обрезан до нулевой длины при открытии.
2. `StandardCopyOption`: Перечисление, предоставляющее стандартные опции для копирования файлов. Некоторые из наиболее распространенных значения включают:
    - `REPLACE_EXISTING`: Если целевой файл уже существует, он будет заменен.
3. `LinkOption`: Перечисление, предоставляющее опции для работы с символическими ссылками. Некоторые из наиболее распространенных значений включают:
    - `NOFOLLOW_LINKS`: Символические ссылки не будут разыменованы.

Эти опции могут быть переданы в методы, такие как `Files.newInputStream()`, `Files.newOutputStream()`, `Files.copy()` и другие, для настройки поведения операций открытия и копирования файлов.

Пример:

```Java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;
import java.io.IOException;

public class OpenOptionExample {
    public static void main(String[] args) {
        Path filePath = Path.of("path/to/file.txt");
        
        try {
            // Открытие файла для чтения и записи
            Files.newOutputStream(filePath, StandardOpenOption.READ, StandardOpenOption.WRITE);
            
            // Дополнительные операции...
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

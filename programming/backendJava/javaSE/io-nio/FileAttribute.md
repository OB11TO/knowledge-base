---
title: FileAttribute
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:39
modified: 2024-09-10T18:40:04+03:00
questions: 
notes: 
links: 
---
### FileAttribute

Атрибуты файла представляют метаданные или свойства файла, такие как разрешения доступа, дата создания, размер и т. д. Они могут использоваться при создании или модификации файлов с помощью классов из пакета `java.nio.file`.

Вместо определенного интерфейса `FileAttribute` в Java API, вы можете определять свои собственные атрибуты файла, реализуя соответствующие интерфейсы и классы из пакета `java.nio.file.attribute`, такие как `BasicFileAttributes`, `PosixFileAttributes`, `DosFileAttributes` и т. д. Эти интерфейсы предоставляют методы для доступа к различным атрибутам файлов, в зависимости от поддержки конкретной файловой системы.

Пример создания и установки атрибутов файла может выглядеть следующим образом:
```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.attribute.FileAttribute;
import java.nio.file.attribute.PosixFilePermissions;

public class FileAttributesExample {
    public static void main(String[] args) throws IOException {
        // Создание нового файла с атрибутами
        Path filePath = Paths.get("path/to/file.txt");
        FileAttribute<?>[] attributes = { 
            PosixFilePermissions.asFileAttribute(PosixFilePermissions.fromString("rw-r--r--"))
        };
        Files.createFile(filePath, attributes);
        
        // Получение атрибутов существующего файла
        BasicFileAttributes fileAttributes = Files.readAttributes(filePath, BasicFileAttributes.class);
        System.out.println("Size: " + fileAttributes.size());
        System.out.println("Creation Time: " + fileAttributes.creationTime());
        System.out.println("Last Modified Time: " + fileAttributes.lastModifiedTime());
        System.out.println("Is Directory: " + fileAttributes.isDirectory());
        // и так далее...
    }
}
```

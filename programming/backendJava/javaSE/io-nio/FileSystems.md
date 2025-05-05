---
title: FileSystems
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:39
modified: 2025-01-20T13:40:59+03:00
questions: 
notes: 
links: 
---

### FileSystems

Класс `FileSystems` в пакете `java.nio.file` предоставляет статические методы для создания объектов `FileSystem` и получения доступа к файловым системам.

Методы:

1. `getDefault()`: Возвращает объект `FileSystem` для работы с файловой системой по умолчанию.
    
2. `getFileSystem(URI uri)`: Возвращает объект `FileSystem` для заданного URI. Этот метод позволяет получить доступ к файловой системе, представленной указанным URI.
```java
URI uri = new URI("file:///path/to/file.txt");
FileSystem fileSystem = FileSystems.getFileSystem(uri);

```
4. `newFileSystem(Path path, Map<String, ?> env)`: Создает новый объект `FileSystem` для указанного пути. Дополнительные параметры окружения могут быть переданы через `Map`.
    
5. `newFileSystem(Path path, ClassLoader loader)`: Создает новый объект `FileSystem` для указанного пути с использованием указанного загрузчика классов.
    
6. `newFileSystem(URI uri, Map<String, ?> env, ClassLoader loader)`: Создает новый объект `FileSystem` для указанного URI с использованием указанного загрузчика классов и дополнительных параметров окружения.

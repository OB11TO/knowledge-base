---
title: Path
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:29
modified: 2024-09-10T18:32:05+03:00
questions: 
notes: 
links: 
---
Класс `Path` в пакете `java.nio.file` представляет путь к файлу или директории в файловой системе. Он предоставляет методы для работы с путями, обеспечивая удобный способ выполнения операций с файлами и директориями независимо от операционной системы.

Основные методы:

1. `getFileName()`: Возвращает имя файла или директории в виде объекта `Path`.
2. `getParent()`: Возвращает родительскую директорию в виде объекта `Path`.
3. `getRoot()`: Возвращает корневую директорию в виде объекта `Path`.
4. `resolve(Path other)`: Разрешает путь относительно другого пути.
5. `resolve(String other)`: Разрешает путь относительно строки.
6. `relativize(Path other)`: Вычисляет относительный путь между двумя путями.
7. `normalize()`: Нормализует путь, удаляя точки и двойные точки.
8. `toAbsolutePath()`: Преобразует путь в абсолютный путь.
9. `startsWith(Path other)`: Проверяет, начинается ли данный путь с указанного пути.
10. `endsWith(Path other)`: Проверяет, заканчивается ли данный путь указанным путем.
11. `getName(int index)`: Возвращает компонент пути по заданному индексу.
12. `getNameCount()`: Возвращает количество компонентов пути.
13. `subpath(int beginIndex, int endIndex)`: Возвращает подпуть между заданными индексами.


Вот некоторые особенности класса `Path`:

1. Создание объекта `Path`: Объекты `Path` могут быть созданы с использованием статического метода `of()` или с помощью методов класса `Paths`. Например:

```Java
Path path1 = Path.of("path/to/file.txt");
Path path2 = Paths.get("path", "to", "file.txt");
```

2. Работа с компонентами пути: Класс `Path` предоставляет методы для получения различных компонентов пути, таких как имя файла, родительская директория, а также методы для добавления, удаления и изменения компонентов пути. Например:

```Java
Path path = Path.of("path/to/file.txt");
System.out.println(path.getFileName()); // Выводит "file.txt"
System.out.println(path.getParent()); // Выводит "path/to"
Path newPath = path.resolveSibling("newfile.txt"); // Создает новый путь "path/to/newfile.txt"
```

3. Разрешение пути: Класс `Path` предоставляет методы для разрешения пути относительно другого пути, а также для нормализации пути. Например:

```Java
Path basePath = Path.of("path/to");
Path relativePath = Path.of("file.txt");
Path resolvedPath = basePath.resolve(relativePath); // Создает новый путь "path/to/file.txt"
Path normalizedPath = resolvedPath.normalize(); // Нормализует путь (удаляет ".." и "."), если они есть
```

4. Проверка существования и типа пути: Класс `Path` предоставляет методы для проверки существования пути, а также для определения его типа, такого как файл, директория или символическая ссылка. Например:

```Java
Path path = Path.of("path/to/file.txt");
System.out.println(Files.exists(path)); // Проверяет, существует ли файл или директория
System.out.println(Files.isDirectory(path)); // Проверяет, является ли путь директорией
System.out.println(Files.isRegularFile(path)); // Проверяет, является ли путь обычным файлом
System.out.println(Files.isSymbolicLink(path)); // Проверяет, является ли путь символической ссылкой
```

5. Итерация по компонентам пути: Класс `Path` предоставляет методы для итерации по компонентам пути, таким как корневой элемент, имена директорий и имя файла. Например:

```Java
Path path = Path.of("path/to/file.txt");
for (Path component : path) {
    System.out.println(component);
}
```

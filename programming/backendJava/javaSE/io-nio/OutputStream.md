---
title: OutputStream
tags:
  - IO-NIO
related_topics: []
created: 2024-09-10 16:06
modified: 2024-09-12T15:56:40+03:00
questions: 
notes: 
links: 
---

----
[[FileOutputStream]]
[[ByteArrayOutputStream]]
[[BufferedOutputStream]]
[[DataOutputStream]]
[[FilterOutputStream]]
[[ObjectOutputStream]]
[[PipedOutputStream]]
[[PrintStream]]
[[SequenceOutputStream]]
[[PushbackOutputStream]]

-----
Каждый из этих классов предоставляет различные способы работы с байтовыми потоками в Java:

1. **`OutputStream`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков вывода байтов.
2. **`FileOutputStream`** —<mark class="hltr-purple"> запись байтов в файл.</mark>
3. **`ByteArrayOutputStream`** — запись <mark class="hltr-blue">байтов в массив байтов в памяти</mark>.
4. **`BufferedOutputStream`** — <mark class="hltr-purple">буферизированная запись</mark> для повышения производительности.
5. **`DataOutputStream`** — запись <mark class="hltr-blue">примитивных типов данных в байтовый поток</mark>.
6. **`FilterOutputStream`** — базовый класс для <mark class="hltr-purple">потоков-декораторов.</mark>
7. **`ObjectOutputStream`** — <mark class="hltr-purple">сериализация</mark> объектов.
8. **`PipedOutputStream`** — <mark class="hltr-blue">межпотоковая</mark> запись данных через каналы.
9. **`PrintStream`** — <mark class="hltr-red">удобная запись строк и данных с методами</mark> `print()` и `println()`.
10. **`SequenceOutputStream`** — <mark class="hltr-purple">объединение нескольких потоков вывода в один.</mark>


Для работы с потоками вывода, которые обрабатывают данные в виде **байтов**, Java предоставляет несколько базовых классов, находящихся в пакете `java.io`. Эти классы используются для записи данных в различные источники: файлы, массивы байтов, сетевые соединения и т.д.

Вот краткое описание каждого базового класса потоков вывода:

---

### 1. **`OutputStream`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый класс для всех потоков вывода, которые работают с байтами.
- **Методы**:
    - `void write(int b)`: Записывает один байт данных.
    - `void write(byte[] b)`: Записывает массив байтов.
    - `void flush()`: Принудительно записывает данные из буфера (если используется буферизация).
    - `void close()`: Закрывает поток и освобождает все ресурсы.
- **Использование**: Основной класс, от которого наследуются все остальные классы вывода.

---

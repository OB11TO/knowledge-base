---
title: InputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 16:05
modified: 2024-09-12T15:56:06+03:00
questions: 
notes: 
links: 
---

----
[[FileInputStream]]
[[ByteArrayInputStream]]
[[BufferedInputStream]]
[[DataInputStream]]
[[FilterInputStream]]
[[ObjectInputStream]]
[[PipedInputStream]]
[[SequenceInputStream]]
[[PushbackInputStream]]

----


Каждый из этих классов предоставляет уникальные возможности для работы с байтовыми потоками:

1. **`InputStream`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков ввода, работающих с байтами.
2. **`FileInputStream`** — для <mark class="hltr-blue">чтения данных из файлов.</mark>
3. **`ByteArrayInputStream`** — для <mark class="hltr-blue">чтения данных из массива байтов</mark>.
4. **`BufferedInputStream`** — <mark class="hltr-blue">добавляет буферизацию</mark> для повышения производительности.
5. **`DataInputStream`** — позволяет<mark class="hltr-blue"> читать примитивные типы данных</mark>.
6. **`FilterInputStream`** — базовый <mark class="hltr-purple">класс для потоков-декораторов</mark>.
7. **`ObjectInputStream`** — для <mark class="hltr-blue">десериализации объектов</mark>.
8. **`PipedInputStream`** — для <mark class="hltr-blue">межпотоковой</mark> передачи данных.
9. **`SequenceInputStream`** — для п<mark class="hltr-purple">оследовательного чтения из нескольких потоков</mark>.
10. **`PushbackInputStream`** — позволяет <mark class="hltr-blue">возвращать байты в поток для повторного чтения</mark>.

Эти классы покрывают широкий спектр задач по работе с байтовыми потоками в Java.

### Примеры:
### 1. **`InputStream`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый <mark class="hltr-blue">класс для всех потоков ввода, которые работают с байтами.</mark> Все <mark class="hltr-yellow">остальные классы потоков ввода наследуются от этого класса.</mark>
- **Методы**:
    - `int read()`: Читает один байт данных, возвращает -1, если достигнут конец потока.
    - `int read(byte[] b)`: Читает несколько байт в массив.
    - `int available()`: Возвращает количество доступных для чтения байт.
    - `void close()`: Закрывает поток и освобождает ресурсы.
- **Использование**: Является базой для специализированных классов потоков.

![[images/Untitled 161.png|Untitled 161.png]]
![[images/Untitled 7 17.png|Untitled 7 17.png]]
---

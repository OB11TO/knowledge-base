---
title: Input-Output
tags:
  - IO-NIO
related_topics: []
created: 2024-09-10 15:27
modified: 2024-09-12T15:56:49+03:00
questions: 
notes: 
links: 
---

---
###### InputStream
[[InputStream]]
###### OutputStream
[[OutputStream]]
###### Reader
[[Reader]]
###### Writer
[[Writer]]

---
### Input-Output

Пакет `java.io` обеспечивает ввод и вывод через потоки (stream) данных.

![[images/Untitled 1 4.png|Untitled 1 4.png]]

Он реализует две группы абстрактных классов:
[[InputStream]] и [[OutputStream]]
##### `InputStream` и `OutputStream` - потоки побайтового ввода-вывода;

![[images/Untitled 2 3.png|Untitled 2 3.png]]

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


[[Reader]] и [[Writer]]
##### `Reader` и `Writer` - потоки посимвольного ввода-вывода.

![[images/Untitled 3 3.png|Untitled 3 3.png]]

Для работы с символами Java предоставляет несколько классов ввода и вывода:

1. **`Reader`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков ввода символов.
2. **`FileReader`** — <mark class="hltr-purple">чтение символов из файла</mark>.
3. **`BufferedReader`** — <mark class="hltr-purple">буферизированное</mark> чтение для повышения производительности.
4. **`CharArrayReader`** — <mark class="hltr-blue">чтение символов из массива символов</mark>.
5. **`StringReader`** — <mark class="hltr-blue">чтение символов из строки.</mark>
6. **`PushbackReader`** — позволяет в<mark class="hltr-blue">озвращать символы обратно в поток.</mark>
7. **`FilterReader`** — базовый класс для потоков-<mark class="hltr-yellow">декораторов</mark>.
8. **`LineNumberReader`** — <mark class="hltr-purple">отслеживание номеров строк при чтении.</mark>

Для записи символов в Java:

1. **`Writer`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков вывода символов.
2. **`FileWriter`** —<mark class="hltr-purple"> запись символов в файл.</mark>
3. **`BufferedWriter`** — <mark class="hltr-purple">буферизированная</mark> запись для повышения производительности.
4. **`CharArrayWriter`** — <mark class="hltr-blue">запись символов в массив символов в памяти</mark>.
5. **`StringWriter`** — <mark class="hltr-blue">запись символов в строку.</mark>
6. **`PrintWriter`** — <mark class="hltr-yellow">удобное написание строк и других данных.</mark>
7. **`FilterWriter`** — базовый класс для потоков-<mark class="hltr-yellow">декораторов</mark>.
8. **`PipedWriter`** — <mark class="hltr-orange">межпотоковая</mark> запись данных через каналы.

Эти классы предоставляют гибкие и мощные средства для работы с текстовыми и бинарными данными в Java.


![[images/Untitled 4 3.png|Untitled 4 3.png]]
![[images/Untitled 9 2.png|Untitled 9 2.png]]
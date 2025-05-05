---
title: Reader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 16:26
modified: 2024-09-12T15:57:10+03:00
questions: 
notes: 
links: 
---

---
[[FileReader]]
[[BufferedReader]]
[[CharArrayReader]]
[[StringReader]]
[[PushbackReader]]
[[FilterReader]]
[[LineNumberReader]]
[[InputStreamReader]]

---
## Краткое описание
Для работы с символами Java предоставляет несколько классов ввода и вывода:

1. **`Reader`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков ввода символов.
2. **`FileReader`** — <mark class="hltr-purple">чтение символов из файла</mark>.
3. **`BufferedReader`** — <mark class="hltr-purple">буферизированное</mark> чтение для повышения производительности.
4. **`CharArrayReader`** — <mark class="hltr-blue">чтение символов из массива символов</mark>.
5. **`StringReader`** — <mark class="hltr-blue">чтение символов из строки.</mark>
6. **`PushbackReader`** — позволяет в<mark class="hltr-blue">озвращать символы обратно в поток.</mark>
7. **`FilterReader`** — базовый класс для потоков-<mark class="hltr-yellow">декораторов</mark>.
8. **`LineNumberReader`** — <mark class="hltr-purple">отслеживание номеров строк при чтении.</mark>
9. **`InputStreamReader`** -преобразует <mark class="hltr-yellow">байтовой поток в символьный поток путем чтения байтов и декодирования их в символы</mark> с использованием определенной кодировки

## Полное описание
#### Классы для работы с символами:

---

### 1. **`Reader`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый класс для всех потоков ввода, работающих с символами (16-битные Unicode символы).
- **Методы**:
    - `int read()`: Читает один символ данных, возвращает -1, если достигнут конец потока.
    - `int read(char[] cbuf)`: Читает несколько символов в массив.
    - `long skip(long n)`: Пропускает указанное количество символов.
    - `void close()`: Закрывает поток и освобождает ресурсы.
- **Использование**: Является базовым классом для всех потоков ввода символов.

---

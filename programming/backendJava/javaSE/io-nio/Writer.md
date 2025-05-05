---
title: Writer
tags:
  - IO-NIO
related_topics: []
created: 2024-09-10 16:28
modified: 2024-09-12T15:57:33+03:00
questions: 
notes: 
links: 
---

---
[[FileWriter]]
[[BufferedWriter]]
[[CharArrayWriter]]
[[StringWriter]]
[[PrintWriter]]
[[FilterWriter]]
[[PipedWriter]]

----
## Краткое описание
Для записи символов в Java:

1. **`Writer`** — <mark class="hltr-yellow">базовый</mark> класс для всех потоков вывода символов.
2. **`FileWriter`** —<mark class="hltr-purple"> запись символов в файл.</mark>
3. **`BufferedWriter`** — <mark class="hltr-purple">буферизированная</mark> запись для повышения производительности.
4. **`CharArrayWriter`** — <mark class="hltr-blue">запись символов в массив символов в памяти</mark>.
5. **`StringWriter`** — <mark class="hltr-blue">запись символов в строку.</mark>
6. **`PrintWriter`** — <mark class="hltr-yellow">удобное написание строк и других данных.</mark>
7. **`FilterWriter`** — базовый класс для потоков-<mark class="hltr-yellow">декораторов</mark>.
8. **`PipedWriter`** — <mark class="hltr-orange">межпотоковая</mark> запись данных через каналы.


## Полное описание
#### Классы для работы с символами:

---

### 1. **`Writer`**

- **Описание**: <mark class="hltr-red">Абстрактный</mark> базовый класс для всех потоков вывода, работающих с символами.
- **Методы**:
    - `void write(int c)`: Записывает один символ.
    - `void write(char[] cbuf)`: Записывает массив символов.
    - `void write(String str)`: Записывает строку.
    - `void flush()`: Принудительно записывает данные из буфера (если используется буферизация).
    - `void close()`: Закрывает поток и освобождает ресурсы.
- **Использование**: Основной класс, от которого наследуются все классы вывода символов.

---

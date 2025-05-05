---
title: Отличия IO от NIO
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 15:33
modified: 2024-09-11T11:54:25+03:00
questions: 
notes: 
links: 
---
### Отличия IO от NIO

-<mark class="hltr-yellow"> Поток I/O блокирует файл</mark>(и thread, который вызвал операцию чтения или записи) <mark class="hltr-yellow">до конца работы с ним</mark>. Нельзя одновременно считывать данные из файла и записывать что-то в него, в то же время thread ждет конца операции, и не идет дальше;
- В пакете<mark class="hltr-yellow"> IO не реализована поддержка файловых систем</mark> (на Windows путь выглядит примерно так: "C:\Users\Vladimir\java\file.txt”, на UNIX-like так: “/users/Vladimir/java/file.txt”) и поддержка ссылок;
- В <mark class="hltr-yellow">IO большое количество проверяемых исключений</mark>
- <mark class="hltr-green2">NIO может напрямую получать атрибуты файла</mark>, например, его размер;
- В <mark class="hltr-green2">NIO реализованы методы для управления файлами</mark> (например, копирования);
- <mark class="hltr-green2">NIO поддерживает асинхронный поток данных.</mark>

![[images/Untitled 5 19.png|Untitled 5 19.png]]
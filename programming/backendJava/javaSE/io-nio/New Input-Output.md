---
title: New Input-Output
tags:
  - IO-NIO
related_topics: []
created: 2024-09-10 15:30
modified: 2024-09-16T13:27:52+03:00
questions: 
notes: 
links: 
---

---
[[Buffer]]

[[Channel]]

[[FileLock]]
[[Selector]]
[[SelectionKey]]

---

### New Input-Output

`Java NIO` - альтернативная библиотека для работы с вводом-выводом.  
Основные принципы NIO:  
1. <mark class="hltr-yellow">Не блокирует операции ввода-вывода: thread получает те данные из буфера, что в нем есть </mark>и переходит к исполнению дальнейших инструкций. Таким образом, например, <mark class="hltr-green2">thread может одновременно получать данные из нескольких каналов (а не по очереди).</mark>  
2. Буфер-ориентированность:<mark class="hltr-yellow"> работа с данными идет не побайтово/посимвольно, а с буфером, который является контейнером для массива примитивного типа данных. </mark> 

![[images/Untitled 5 3.png|Untitled 5 3.png]]

`Java NIO` <mark class="hltr-orange">базируется на следующих компонентах:</mark>

- <mark class="hltr-red">Абстрактный класс </mark>`Buffer`  имеет наследников для работы с разными типами данных, например, `ByteBuffer`. В нем <mark class="hltr-yellow">содержатся данные, которые можно будет обработать, используя каналы.</mark>

![[images/Untitled 6 2.png|Untitled 6 2.png]]

- <mark class="hltr-red">Интерфейс</mark> `Channel` - новый тип абстракции, похожий на Stream. В <mark class="hltr-green2">отличие от потока канал является двусторонним, и может одновременно считывать и записывать данные в буфер. Также поддерживает режим блокировки</mark>

![[images/Untitled 7 2.png|Untitled 7 2.png]]

- <mark class="hltr-red">Класс</mark> `Selector` - объект, <mark class="hltr-yellow">отслеживающий события в нескольких каналах</mark>. <mark class="hltr-green2">При неблокирующих IO-операциях, селектор используется для выбора каналов, готовых к вводу/выводу.</mark>

![[images/Untitled 8 2.png|Untitled 8 2.png]]

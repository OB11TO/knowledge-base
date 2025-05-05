---
title: Андрей Паньгин — Искусство Java профилирования
tags:
  - JVM
related_topics: 
created: 2025-01-31 16:16
modified: 2025-01-31T16:34:38+03:00
questions: 
notes: 
links:
  - https://www.youtube.com/watch?v=QiGrTvsCZmA
---

![[Pasted image 20250131161702.png]]
### Что такое Safepoint?

<mark class="hltr-red">Safepoint</mark> — это состояние, в котором<mark class="hltr-green2"> все потоки приложения остановлены, чтобы JVM могла выполнить некоторые глобальные операции, такие как сборка мусора, компиляция методов JIT-компилятором или обновление внутренних структур данных.</mark> Когда JVM достигает safepoint, она временно приостанавливает все потоки приложения, чтобы гарантировать, что они находятся в безопасном состоянии для выполнения этих операций.
![[Pasted image 20250131162627.png]]

![[Pasted image 20250131162943.png]]

- <mark class="hltr-red">В Цикле раньше HotSpot не делал safepoint, программа не остановиться.  Поэтому нужно сделал доп. ключик. </mark>

----

![[Pasted image 20250131163154.png]]

 ![[Pasted image 20250131163334.png]]
 
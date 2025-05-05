---
title: Just-In-Time (JIT)
tags:
  - JavaSE
related_topics: 
created: 2025-04-01 14:27
modified: 2025-04-01T14:28:17+03:00
questions: 
notes: 
links: 
---



---



----

В стандартной поставке JVM, такой как Oracle JDK или OpenJDK, по умолчанию используется оптимизирующий JIT компилятор HotSpot. <mark class="hltr-green2">В HotSpot JIT компилятор начинает работать по умолчанию после 10 000 вызовов функции</mark>. Это значение может быть настроено с помощью параметров командной строки JVM, таких как -`XX:CompileThreshold.`


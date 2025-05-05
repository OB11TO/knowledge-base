---
title: Логирование job4j
tags:
  - JavaSE
  - Job4j
related_topics: 
created: 2025-01-22 18:30
modified: 2025-01-22T19:31:50+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20250122183123.png]]


![[Pasted image 20250122185303.png]]
![[Pasted image 20250122185424.png]]



----
### SLF использует шаблон проектирования - фасад. Шаблон фасад упрощает АПИ логгеров. Делает их понятными.


-----

![[Pasted image 20250122190225.png]]
Параметры указываются после шаблона.
Метки заменяются последовательно. Первая метка заменится первым параметром, вторая - вторым и так далее.
Если меток или параметров будет разное количество логгер проигнорирует метку или параметр.

----
#### Правило 
Запомните правило, е<mark class="hltr-red">сли в проекте используется логгер, то для вывода ошибок или отладочной информации нужно использовать только логгер.</mark>
Придерживаетесь единого стиля во всем проекте.
Рассмотрим ситуацию с исключением.
```java
package ru.job4j.io;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class UsageLog4j {

    private static final Logger LOG = LoggerFactory.getLogger(UsageLog4j.class.getName());

    public static void main(String[] args) {
        try {
            throw new Exception("Not supported code");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

В этом примере stack исключения печатается напрямую в консоль. <mark class="hltr-green2">Перенаправим его в логгер.</mark>
```java
package ru.job4j.io;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class UsageLog4j {

    private static final Logger LOG = LoggerFactory.getLogger(UsageLog4j.class.getName());

    public static void main(String[] args) {
        try {
            throw new Exception("Not supported code");
        } catch (Exception e) {
            LOG.error("Exception in log example", e);
        }
    }
}
```


---

![[Pasted image 20250122190906.png]]
![[Pasted image 20250122190950.png]]

-----

![[Pasted image 20250122191650.png]]

https://logging.apache.org/log4j/2.x/manual/appenders.html#

```java
log4j.rootLogger=TRACE, CONSOLE_LOG, FILE_LOG

## Console appender
log4j.appender.CONSOLE_LOG=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE_LOG.Threshold=WARN
log4j.appender.CONSOLE_LOG.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE_LOG.layout.ConversionPattern=%d{ISO8601} %5p %c:%M:%L - %m%n

## File appender
log4j.appender.FILE_LOG=org.apache.log4j.FileAppender
log4j.appender.FILE_LOG.Threshold=DEBUG
log4j.appender.FILE_LOG.file=logs/debug.txt
log4j.appender.FILE_LOG.layout=org.apache.log4j.PatternLayout
log4j.appender.FILE_LOG.layout.ConversionPattern=%d{ISO8601} %5p %c:%M:%L - %m%n
```

![[Pasted image 20250122191734.png]]

---

- <mark class="hltr-red">Если будет INFO, то пойдет от INFO, а не от DEBUG</mark>
![[Pasted image 20250122192900.png]]
![[Pasted image 20250122192944.png]]

- <mark class="hltr-orange">ЕСЛИ ПОЛНОСТЬЮ УБРАТЬ И ОСТАВИТЬ ТОЛЬКО COSOLE_LOG, FILE_LOG, ТО file будет норм, как задумано, а вот console не выведет ничего! </mark>


------


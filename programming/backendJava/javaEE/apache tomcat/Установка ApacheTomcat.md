---
title: Установка ApacheTomcat
tags:
  - ApacheTomcat
  - JavaEE
related_topics: 
created: 2024-09-11 12:59
modified: 2024-09-12T16:07:29+03:00
questions: 
notes: 
links: 
---
### Установка ApacheTomCat:

1. Перейти по ссылке `[https://tomcat.apache.org/download-10.cgi](https://tomcat.apache.org/download-10.cgi)` , скачать архив, разархивировать.
2. Перейти в /bin выполнить следующий код:

```Bash
./startup.sh 
```

Если установка прошла успешно, увидим:

```Bash
Using CATALINA_BASE:   /home/vadim/ApacheTomcat10
Using CATALINA_HOME:   /home/vadim/ApacheTomcat10
Using CATALINA_TMPDIR: /home/vadim/ApacheTomcat10/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /home/vadim/ApacheTomcat10/bin/bootstrap.jar:/home/vadim/ApacheTomcat10/bin/tomcat-juli.jar
Using CATALINA_OPTS:   
Tomcat started.
```

Можно перейти по [http://localhost:8080/](http://localhost:8080/) чтобы увидеть запущенный сервер.

![[images/Untitled 3 6.png|Untitled 3 6.png]]

  
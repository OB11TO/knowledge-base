---
title: Как развернуть приложение на Tomcat
tags:
  - ApacheTomcat
  - JavaEE
related_topics: 
created: 2024-09-11 13:00
modified: 2024-09-12T16:07:13+03:00
questions: 
notes: 
links: 
---
### Развернуть приложение на Tomcat

1. Создать новые проект тип JakartaEE, указать путь к директории Tomcat, если версия Tomcat 10+, Java должна быть 8+.

![[images/Untitled 4 6.png|Untitled 4 6.png]]

  

1. Создать из существующего war архив.
    
    - Files→Project Structure→Artifacts→New module→Web( он будет exploded, разархивированный)
    - После этого Files→Project Structure→Artifacts→New module → Web application Archive → используя из exploded web архива, созданного выше.
    
    ![[images/Untitled 5 6.png|Untitled 5 6.png]]
    
    - После этого переместить архив в папку tomcat/webapps
    - Перезапустить tomcat, он автоматически разахивирует war архив
    
    ![[images/Untitled 6 5.png|Untitled 6 5.png]]
    
      
    
    1. через IDEA
    
    - Edit Configurations→ TomCat
    - После этого необходимо настроить и добавить артефакт( наш разархивированный war архив).
    - ВАЖНО⚠️ если tomcat уже запущен на порту 8080, необходимо его либо отключчить, чтобы запустить также на 8080 либо поменять порт. У меня 8081.
    
    ![[images/Untitled 7 5.png|Untitled 7 5.png]]
    
      
    
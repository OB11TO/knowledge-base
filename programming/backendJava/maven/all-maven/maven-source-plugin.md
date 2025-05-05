---
title: maven-source-plugin
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:42
modified: 2024-09-17T16:45:12+03:00
questions: 
notes: 
links: 
---

### maven-source-plugin

Еще один полезный плагин – `maven-source-plugin` позволяет включать в сборку исходный код ваших java-файлов.

Кроме web-приложений, с помощью Maven собирается  
очень большое количество библиотек. Очень много Java-проектов следуют  
концепции open-source и распространяются среди Java-сообщества со своими  
исходниками.  

В любом сложном проекте исходники могут храниться в нескольких местах.

Во-вторых, часто используется генерация исходников на основе xml-спецификаций, такие исходники тоже нужно включать в сборку.

Ну и в-третьих, ты можешь решить не включать какие-нибудь особо секретные файлы в вашу сборку.

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>2.2.1</version>
    <executions>
        <execution>
            <id>attach-sources</id>
            <phase>verify</phase>
            <goals>
                <goal>jar</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

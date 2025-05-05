---
title: Jacoco maven plugin
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:45
modified: 2024-09-16T17:45:45+03:00
questions: 
notes: 
links: 
---

### Jacoco maven plugin

[https://www.eclemma.org/jacoco/trunk/doc/maven.html](https://www.eclemma.org/jacoco/trunk/doc/maven.html)

Плагин используется для создания отчета прохождения тестов.

```XML
<plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.6</version>
        <executions>
          <execution>
            <id>prepare-agent</id>
            <goals>
              <goal>prepare-agent</goal>
            </goals>
          </execution>
          <execution>
            <id>generate-report-jacoco</id>
            <goals>
              <goal>report</goal>
            </goals>
            <phase>prepare-package</phase>
          </execution>
        </executions>
      </plugin>
```

После выполения фазы pakage создает в папке target/jacoco.exec. После создания документации с помощью goal site можно посмотреть в папке jacoco страницу html
![[images/Untitled 12 7.png|Untitled 12 7.png]]
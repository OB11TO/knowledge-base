---
title: Jar War
tags:
  - Gradle
related_topics: 
created: 2024-09-23 12:10
modified: 2024-09-23T12:24:52+03:00
questions: 
notes: 
links: 
---

- <mark class="hltr-red">Manifest</mark> - это<mark class="hltr-green2"> конфигурирующий файл для jar</mark>
![[Pasted image 20240923121120.png]]

- Нужно <mark class="hltr-yellow">указать Main класс </mark>
![[Pasted image 20240923121319.png]]
- Нужно <mark class="hltr-yellow">сконфигурировать gradle </mark>
![[Pasted image 20240923121402.png]] 

- Теперь <mark class="hltr-yellow">сделать нужно fat jar</mark>
![[Pasted image 20240923122153.png]]

##  **Создание отдельной задачи**
Если мы хотим <mark class="hltr-yellow">оставить исходную задачу jar как есть</mark>, мы <mark class="hltr-green2">можем создать отдельную задачу,</mark> которая будет выполнять ту же работу.
Следующий код добавит новую задачу с именем <mark class="hltr-yellow">customFatJar</mark>:

```groovy
task customFatJar(type: Jar) {
    manifest {
        attributes 'Main-Class': 'com.baeldung.fatjar.Application'
    }
    archiveBaseName = 'all-in-one-jar'
    duplicatesStrategy = DuplicatesStrategy.EXCLUDE
    from { configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) } }
    with jar
}
```

## **Использование специальных плагинов**
 Использовать <mark class="hltr-green2">плагин</mark> [Shadow](https://github.com/johnrengelman/shadow) :
```java
 plugins {
  id 'com.github.johnrengelman.shadow' version '7.1.2'
  id 'java'
}
```

 или 
```java
buildscript {
    repositories {
        mavenCentral()
        gradlePluginPortal()
    }
    dependencies {
        classpath "gradle.plugin.com.github.johnrengelman:shadow:7.1.2"
    }
}

```



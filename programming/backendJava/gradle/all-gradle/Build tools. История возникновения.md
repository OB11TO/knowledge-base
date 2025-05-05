---
title: Build tools. История возникновения
tags:
  - Gradle
related_topics: 
created: 2024-09-16 18:28
modified: 2024-09-18T14:29:30+03:00
questions: 
notes: 
links: 
---

## Build tools. История возникновения

![[images/Untitled 20 4.png|Untitled 20 4.png]]

![[images/Untitled 21 4.png|Untitled 21 4.png]]

![[images/Untitled 22 4.png|Untitled 22 4.png]]

##  Gradle Object Model

Интерфейсы <mark class="hltr-red">Gradle Settings Project Task Action Script</mark>

- Под капотом считывается build scripts и строится модель
- <mark class="hltr-purple">Settings</mark>  ==для конфигурации== `settings.gradle` и служит для ==конфигурации иерархии== <mark class="hltr-green2">Project</mark> ==объектов.==
- <mark class="hltr-purple">Project</mark>  служит <mark class="hltr-yellow">для конфигурации</mark> `build.gradle`
- <mark class="hltr-purple">Task</mark> для ==описания задачи== в файле.
- <mark class="hltr-purple">Action</mark>  все ==таски разбиваются на части==.
- <mark class="hltr-purple">Script</mark>  описывает все скрипты, которые выше. Создает их.

 
---
title: Gradle Lifecycle
tags:
  - Gradle
related_topics: 
created: 2024-09-16 18:29
modified: 2024-09-18T15:21:59+03:00
questions: 
notes: 
links: 
---

----
[[Gradle object]]
[[Settings object]]
[[Project object]]
[[Task object]]
[[Action object]]

----

### Gradle Lifecycle

![[images/Untitled 23 4.png|Untitled 23 4.png]]

**Gradle кроме принципа "convention over configuration" также заимствовал в Apache Maven понятие жизненного цикла (Lifecycle). 
Только использует он Lifecycle не для привязки своих задач к фазам, а для конфигурации объектной модели и последующий их запуск. Всего существует 3 фазы:  
- initialization (фаза инициализации)  
- configuration (фаза настройки)  
- execution (фаза выполнения)  
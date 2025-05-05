---
title: Dependencies
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:37
modified: 2024-09-16T17:37:36+03:00
questions: 
notes: 
links: 
---

---
[[Области видимости dependencies(scope)]]
[[Добавление стороннего repository в pom.xml]]
[[Транзитивные зависимости]]

---

### Dependencies

![[images/Untitled 5 10.png|Untitled 5 10.png]]

Если хочешь ==добавить в свой Maven-проект какую-нибудь библиотеку==, тебе просто нужно добавить ее в pom-файл, в раздел dependencies.

```XML
<dependencies>
 
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
	<version>5.3.18</version> 
  </dependency>

  <dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>6.0.0.Final</version>
  </dependency>

</dependencies>
```

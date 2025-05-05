---
title: Области видимости dependencies(scope)
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:38
modified: 2024-11-07T16:53:02+03:00
questions: 
notes: 
links: 
---

### Области видимости dependencies(scope)

<mark class="hltr-red">compile</mark> : Зависимый эффективный <mark class="hltr-yellow">диапазон по  
умолчанию</mark>   Если эффективная область зависимости не указана явно при  
определении зависимости, по умолчанию принимается действующая область  
зависимости. <mark class="hltr-green2">Такая зависимость  
допустима при компиляции, запуске и  тестировании.</mark>

<mark class="hltr-red">provided</mark> : Он <mark class="hltr-green2">действителен во время компиляции и  
тестирования</mark>, <mark class="hltr-red">но недействителен во время выполнения  </mark>
Например:  
servlet-api, когда проект запущен, контейнер уже предоставлен, поэтому  
Maven не нужно вводить его снова и снова.  

<mark class="hltr-red">runtime</mark> : Действителен <mark class="hltr-green2">при запуске и тестировании</mark>,  
но <mark class="hltr-red">недействителен при компиляции кода.  </mark>

Например: реализация <mark class="hltr-yellow">драйвера  
JDBC</mark>, компиляция кода проекта требует только интерфейса JDBC,  
предоставляемого JDK, а конкретный драйвер JDBC, реализующий  
вышеуказанный интерфейс, требуется только при тестировании или запуске  
проекта.  

<mark class="hltr-red">test</mark> : Действительно <mark class="hltr-green2">только во время тестирования</mark> например: JUnit.

```XML
<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>3.2.1.RELEASE</version>
            <scope>test</scope>
        </dependency>
```

<mark class="hltr-red">system</mark> : Он действителен <mark class="hltr-green2">во время компиляции и  
тестирования</mark>, но<mark class="hltr-red"> недействителен во время выполнения.  </mark>

`Отличие от provided`  
заключается в том, что при использовании зависимостей на <mark class="hltr-yellow">уровне системы  
путь к зависимому файлу должен быть явно указан</mark> через элемент  `systemPath`. Поскольку такие зависимости не разрешаются через репозиторий  Maven и часто привязаны к собственной системе, что может сделать сборку  
непереносимой, ее следует использовать с осторожностью. Элемент  
systemPath может ссылаться на переменные среды. Например:  

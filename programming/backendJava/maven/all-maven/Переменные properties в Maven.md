---
title: Переменные properties в Maven
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:36
modified: 2024-09-17T14:27:48+03:00
questions: 
notes: 
links: 
---

### Переменные properties в Maven

<mark class="hltr-yellow">Часто встречающиеся параметры Maven</mark> позволяет вынести в переменные.  
Это очень полезно, когда нужно чтобы в разных частях pom-файла параметры  
совпадали. Например, в переменную можно вынести версию Java, версии  
библиотеки, пути к определенным ресурсам.  

Для этого есть специальная секция в `pom.xml – <properties>`, в ней и объявляются переменные. Общий вид переменной такой:

`<имя-переменной>значение</имя-переменной>`

Пример:

```XML
<properties>
    <junit.version>5.2</junit.version>
    <project.artifactId>new-app</project.artifactId>
    <maven.compiler.source>1.13</maven.compiler.source>
    <maven.compiler.target>1.15</maven.compiler.target>
</properties>
```

Для обращения к переменным используется другой синтаксис:

`${имя-переменной}`

Там, где написан такой код, Maven подставит значение переменной.

Пример:

```XML
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <finalName>${project.artifactId}</finalName>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>2.3.2</version>
        <configuration>
            <source>${maven.compiler.source}</source>
            <target>${maven.compiler.target}</target>
        </configuration>
    </plugin>
</build>
```

При описании проекта в pom-файле можно использовать предопределенные переменные. Их можно условно разделить на несколько групп:

- Встроенные свойства проекта;
- Свойства проекта;
- Настройки.

Встроенных свойств проекта всего два:

|   |   |
|---|---|
|Свойство|Описание|
|${basedir}|корневой каталог проекта, где располагается `pom.xml`|
|${version}|версия артефакта; можно использовать `${project.version}` или `${pom.version}`|

На свойства проекта можно ссылаться с помощью префиксов `«project»` или `«pom»`. Их у нас четыре:

|   |   |
|---|---|
|Свойство|Описание|
|${project.build.directory}|`«target»` директория проекта|
|${project.build.outputDirectory}|`«target»` директория компилятора. По умолчанию `«target/classes»`|
|${project.name}|наименование проекта|
|${project.version}|версия проекта|

Доступ к свойствам `settings.xml` можно получить с помощью префикса `settings`. Имена могут быть любыми – берутся из `settings.xml`. Пример:

```Plain
${settings.localRepository} задает путь к локальному репозиторию.
```

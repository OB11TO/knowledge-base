---
title: maven-dependency-plugin
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:42
modified: 2024-09-17T16:45:36+03:00
questions: 
notes: 
links: 
---

### maven-dependency-plugin

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>2.5.1</version>
    <configuration>
        <outputDirectory>
            ${project.build.directory}/lib/
        </outputDirectory>
    </configuration>
    <executions>
        <execution>
            <id>copy-dependencies</id>
            <phase>package</phase>
            <goals>
                <goal>copy-dependencies</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

В этом примере прописано дефолтное поведение плагина – копирование библиотек в директорию `${project.build.directory}/lib`.

В секции execution прописано, что **плагин будет вызван во время фазы сборки – package**, goal – copy-dependences.

В целом, у этого плагина довольно большой набор целей, вот самые популярные из них:

|   |   |   |
|---|---|---|
||||
|1|dependency:analyze|анализ зависимостей (используемые, неиспользуемые, указанные, неуказанные)|
|2|dependency:analyze-duplicate|определение дублирующихся зависимостей|
|3|dependency:resolve|разрешение (определение) всех зависимостей|
|4|dependency:resolve-plugin|разрешение (определение) всех плагинов|
|5|dependency:tree|вывод на экран дерева зависимостей|

Также в разделе configuration можно задать дополнительные параметры:

|   |   |   |
|---|---|---|
||||
|1|outputDirectory|Директория, в которую будут копироваться зависимости|
|2|overWriteReleases|Флаг необходимости перезаписывания зависимостей при создании релиза|
|3|overWriteSnapshots|Флаг необходимости перезаписывания неокончательных зависимостей, в которых присутствует SNAPSHOT|
|4|overWriteIfNewer|Флаг необходимости перезаписывания библиотек с наличием более новых версий|

Пример:

```XML

<configuration>
    <outputDirectory>
         ${project.build.directory}/lib/
    </outputDirectory>
    <overWriteReleases>false</overWriteReleases>
    <overWriteSnapshots>false</overWriteSnapshots>
    <overWriteIfNewer>true</overWriteIfNewer>
 </configuration>
```

  
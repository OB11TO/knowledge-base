---
title: Секция build в pom.xml
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:36
modified: 2024-09-17T14:26:25+03:00
questions: 
notes: 
links: 
---

### Секция build в pom.xml

<mark class="hltr-yellow">Секция build не является обязательной</mark> – <mark class="hltr-red">Maven умеет собирать проект и без нее Но если ты хочешь настроить сборку более-менее сложного проекта, то понимание как там все устроено тебе пригодится.</mark>

Данная секция (ниже) <mark class="hltr-purple">содержит основную информацию по сборке: где расположены Java-файлы, файлы ресурсов, какие плагины используются, куда складывать собранный проект.</mark>

```XML
<build>
        <finalName>projectName</finalName>
        <sourceDirectory>${basedir}/src/java</sourceDirectory>
        <outputDirectory>${basedir}/targetDir</outputDirectory>
        <resources>
                <resource>
                <directory>${basedir}/src/java/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                </includes>
                </resource>
        </resources>
        <plugins>
                . . .
        </plugins>
    </build>
```

Основных тегов тут четыре:

- `<finalName>` ==задает имя результирующего файла сборки== (jar, war, ear..), который создается в фазе package. Если параметр не задан, то используется значение ==по умолчанию — artifactId-version.==
- `<sourceDirectory>` ==позволяет переопределить месторасположения файлов с исходным кодом==. По умолчанию файлы располагаются в директории ${basedir}/src/main/java, но можно указать и любое другое место.
- `<outputDirectory>` ==задает директорию, куда компилятор будет сохранять результаты компиляции== – `*.class файлы`. По умолчанию определено значение target/classes.
- `<resources>` ==вложенные в нее тэги== \<resource> ==определяют местоположение файлов ресурсов. Файлы ресурсов при сборке просто копируются в директорию outputDirectory.== Значение по умолчанию директории с ресурсами равно src/main/resources.

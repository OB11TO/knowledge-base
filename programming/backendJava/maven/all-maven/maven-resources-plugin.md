---
title: maven-resources-plugin
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:42
modified: 2024-09-16T17:42:22+03:00
questions: 
notes: 
links: 
---

### maven-resources-plugin

Если ты собираешь web-приложение, то у тебя будет просто куча  
различных ресурсов в нем. Это jar-библиотеки, jsp-сервлеты, файлы  
настроек. Ну и конечно же это куча статических файлов типа  
`html`, `css`, `js`, а также различных картинок.

По умолчанию Maven при сборке проекта просто скопирует все ваши файлы из папки `src/main/resources` в директорию target. Если же ты хочешь внести изменения в это поведение, то тебе поможет плагин `maven-resources-plugin`.

```XML
<plugin>
    <artifactId>maven-resources-plugin</artifactId>
    <version>2.6</version>
    <executions>
        <execution>
            <id>copy-resources</id>
            <phase>validate</phase>
            <goals>
                <goal>copy-resources</goal>
            </goals>
            <configuration>
                <outputDirectory>
                   ${basedir}/target/resources
                </outputDirectory>
                <resources>
                    <resource>  инструкции по копированию ресурса 1
															 </resource>
                    <resource>  инструкции по копированию ресурса 2
															 </resource>
                    <resource>  инструкции по копированию ресурса N 
																</resource>
                </resources>
            </configuration>
        </execution>
    </executions>
</plugin>
```

**Фильтрация ресурсов**

Ресурсы плагина можно задавать не только в виде файлов, а сразу в  
виде директорий. Более того, к директории можно добавить маску, которая  
задает какие именно файлы из нее будет включены в данный ресурс.  

```XML

            <resource>
                <directory>src/main/resources/images</directory>
                <includes>
                     <include>**/*.png</include>
                </includes>
            </resource>
```

Если ты хочешь исключить какие-нибудь файлы, можешь воспользоваться тегом `exclude`

```XML
<resource>
    <directory>src/main/resources/images</directory>
    <includes>
        <include>**/*.png</include>
    </includes>
    <excludes>
         <exclude>old/*.png</exclude>
    </excludes>
</resource>
```

Плагин умеет заглядывать внутрь файлов (если они текстовые, конечно). И, например, добавить в файл `application.properties` нужную версию сборки. Чтобы плагин обрабатывал содержимое файла, нужно указать ему параметр `<filtering>true</filtering>`.

```XML
<resource>
    <directory>src/main/resources/properties</directory>
    <filtering>true</filtering>
    <includes>
        <include>**/*. properties </include>
    </includes>
</resource>
```

  
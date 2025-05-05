---
title: maven-jar-plugin
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:43
modified: 2024-09-16T17:43:16+03:00
questions: 
notes: 
links: 
---


### maven-jar-plugin

```XML
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>2.4</version>
    <configuration>
        <includes>
            <include>**/properties/*</include>
        </includes>
        <archive>
           <manifestFile>src/main/resources/META-INF/MANIFEST.MF</manifestFile>
        </archive>
    </configuration>
</plugin>
```

с его помощью можно указать, какие файлы попадут в библиотеку, а какие – нет. С помощью тегов `<include>` в секции `<includes>` можно задать список директорий, чей контент нужно добавить в библиотеку.

Во-вторых, каждая jar-библиотека должна иметь манифест (файл **MANIFEST.MF**).  
Плагин сам положит его в нужное место библиотеки, вам всего лишь нужно  
указать, по какому пути его взять. Для этого используется тег  
`<manifestFile>`.

И наконец, плагин может самостоятельно сгенерировать манифест. Для этого вместо тега `<manifestFile>` вам нужно добавить тег `<manifest>` и в нем указать данные для будущего манифеста. Пример:

```XML
<configuration>
    <archive>
        <manifest>
            <addClasspath>true</addClasspath>
            <classpathPrefix>lib/</classpathPrefix>
            <mainClass>ru.javarush.MainApplication</mainClass>
        </manifest>
    </archive>
</configuration>
```

Тег `<addClasspath>` определяет необходимость добавления в манифест `CLASSPATH`.

Тег `<classpathPrefix>` позволяет дописывать префикс (в примере lib) перед каждым ресурсом. Определение префикса в `<classpathPrefix>` позволяет размещать зависимости в отдельной папке. Можно размещать библиотеки внутри другой библиотеки.

И наконец, тег `<mainClass>` указывает на главный исполняемый класс. Java-машина может запустить программу, которая задана не только Java-классом, но и jar-файлом и именно для такого случая нужен главный стартовый класс.

### Плагин генерации номера сборки buildnumber-maven-plugin

Очень часто jar-библиотеки и war-файлы включают в себя информацию с  
названием проекта и его версией, а также версией сборки. Мало того, что  
это полезно для управления зависимостями, так еще и упрощает  
тестирование: понятно, в какой версии библиотеки ошибка исправлена, а в  
какой – добавлена.  

Чаще всего эту задачу решают так – создают специальный файл `application.properties`,  
который содержит всю нужную информацию и включают его в сборку. Так же  
можно настроить сценарий сборки так, чтобы данные из этого файла  
перекочевывали в  
`MANIFEST.MF`.

у Maven есть специальный плагин, который может генерировать такой  
application.properties файл. Для этого нужно создать такой файл и  
заполнить его специальными шаблонами данных. Пример:  

```Plain
# application.properties
app.name=${pom.name}
app.version=${pom.version}
app.build=${buildNumber}
```

Значения всех трех параметров будут подставляться на этапе сборки.

Параметры `pom.name` и `pom.version` будут браться прямо из `pom.xml`. А для генерации уникального номера сборки в Maven есть специальный плагин – `buildnumber-maven-plugin`.

```XML
<packaging>war</packaging>
<version>1.0</version>
<plugins>
    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>buildnumber-maven-plugin</artifactId>
        <version>1.2</version>
        <executions>
            <execution>
                <phase>validate</phase>
                <goals>
                    <goal>create</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <revisionOnScmFailure>true</revisionOnScmFailure>
            <format>{0}-{1,date,yyyyMMdd}</format>
            <items>
                 <item>${project.version}</item>
                 <item>timestamp</item>
            </items>
        </configuration>
    </plugin>
</plugins>
```

В примере выше происходят три важные вещи. Во-первых, указан сам плагин для задания версии сборки. Во-вторых, указано, что он будет запускаться во время фазы validate (самая первая фаза) и генерировать номер сборки – `${buildNumber}`.

И в-третьих, указан формат этого номера сборки, который склеивается из нескольких частей. Это версия проекта `project.version` и текущее время заданное шаблоном. Формат шаблона задается Java-классом `MessageFormat`.

  
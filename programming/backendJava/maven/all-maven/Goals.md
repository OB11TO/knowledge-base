---
title: Goals
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:40
modified: 2024-09-17T15:24:11+03:00
questions: 
notes: 
links: 
---

### Goals

В Maven <mark class="hltr-yellow">есть еще такое понятие как цель</mark> (goal). goal – <mark class="hltr-green2">это как бы  
цель запуска Maven</mark>.  

На деле <mark class="hltr-purple">плагин это Java-проект, который содержит классы (Goals), которые наследуются от AbstractMojo, который в свою очередь имплементирует интерфейс Mojo и переопределяет метод execute.</mark>

```Java
@Mojo( name = "clean", threadSafe = true )
public class CleanMojo extends AbstractMojo
{
@Override
public void execute() throws MojoExecutionException{}
```

![[images/Untitled 9 9.png|Untitled 9 9.png]]
![[Pasted image 20240917152200.png]]
- `Можем вызвать ещё раз, помимо дефолтного.` !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

Если тебе нужно выполнить какие-нибудь <mark class="hltr-yellow">нестандартные действия в определенной фазе, то всего лишь нужно добавить соответствующий плагин</mark> в pom.xml. В <mark class="hltr-yellow">поле</mark> <\execution> <mark class="hltr-yellow">выполнить</mark> <mark class="hltr-green2">goal</mark> и <mark class="hltr-yellow">указать в какой фазе его выполнить</mark>, некоторые goals не привязаны к жизненному циклу, их нужно прописывать вручную. Чтобы узнать какие goals есть у фазы, необходимо вызвать `mvn фаза:help`


```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>имя-плагина</artifactId>
  <executions>
    <execution>
      <id>customTask</id>
      <phase>generate-sources</phase>
      <goals>
        <goal>pluginGoal</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

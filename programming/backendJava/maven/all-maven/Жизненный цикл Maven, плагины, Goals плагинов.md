---
title: Жизненный цикл Maven, плагины, Goals плагинов
tags:
  - Maven
related_topics: 
created: 2024-09-16 17:39
modified: 2024-09-17T15:30:06+03:00
questions: 
notes: 
links: 
---

---
[[Жизненный цикл]]
[[Goals]]
[[Плагины]]

---

### Жизненный цикл Maven, плагины, Goals плагинов

![[images/Untitled 8 9.png|Untitled 8 9.png]]



> [!faq] Могут ли фазы состоять из нескольких целей в maven ?

Да, в Maven фазы (phases) могут включать в себя несколько целей (goals). Более того, это стандартная практика.

### Как это работает:

Maven использует концепцию **жизненного цикла сборки** (build lifecycle), который состоит из фаз. Каждая фаза может запускать одну или несколько целей, ассоциированных с плагинами.

### Основные моменты:

- **Фаза** — это этап в жизненном цикле Maven.
- **Цель** — это конкретное действие, которое выполняется в рамках фазы, например, компиляция, тестирование, упаковка и т.д.
- **Фаза может содержать несколько целей**. При вызове фазы автоматически запускаются все цели, ассоциированные с этой фазой и предшествующими ей фазами.

### Пример:

Рассмотрим стандартный жизненный цикл сборки Maven (`default` lifecycle), который включает такие фазы, как:

- `validate`
- `compile`
- `test`
- `package`
- `install`
- `deploy`

Каждая из этих фаз может иметь несколько целей. Например, фаза `package` может включать:

- Цель `maven-resources-plugin:resources`, которая копирует ресурсы.
- Цель `maven-compiler-plugin:compile`, которая компилирует исходный код.
- Цель `maven-jar-plugin:jar`, которая создает JAR-файл.

### Как цели привязываются к фазам:

Цели (goals) привязываются к фазам с помощью плагинов. Это делается либо автоматически (<mark class="hltr-yellow">плагины по умолчанию ассоциированы с фазами</mark>), либо через конфигурацию в `pom.xml`.

<mark class="hltr-green2">Пример привязки цели к фазе:</mark>

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.8.1</version>
      <executions>
        <execution>
          <goals>
            <goal>compile</goal>
          </goals>
          <phase>compile</phase>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>


```

В данном примере цель `compile` плагина `maven-compiler-plugin` привязана к фазе `compile`.

### Как запускаются фазы и цели:

Когда вы вызываете фазу, Maven запускает все цели, которые привязаны к этой фазе, а также ко всем предыдущим фазам жизненного цикла. Например:

- Если вы запустите команду `mvn package`, Maven выполнит цели, привязанные ко всем фазам до `package` включительно (`validate`, `compile`, `test`, `package`).

### Пример с несколькими целями для одной фазы:

<mark class="hltr-yellow">Если фаза должна запускать несколько целей, это можно настроить с помощью нескольких целей в одной фазе.
</mark>

Пример:
```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-resources-plugin</artifactId>
      <version>3.2.0</version>
      <executions>
        <execution>
          <goals>
            <goal>resources</goal>
            <goal>testResources</goal>
          </goals>
          <phase>process-resources</phase>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>

```

В этом примере фаза `process-resources` выполнит две цели: `resources` и `testResources`.

### Вывод:

Да, в Maven одна фаза может содержать несколько целей. Цели привязываются к фазам через плагины, и при выполнении фазы Maven автоматически запускает все цели, которые с ней связаны.
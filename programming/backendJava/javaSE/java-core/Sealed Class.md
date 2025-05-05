---
title: Sealed Class
tags:
  - JavaSE
related_topics: 
created: 2024-08-31 13:09
modified: 2024-08-31T13:09:56+03:00
difficulty: easy
questions: 
notes: 
links: 
---
# Sealed Class

Sealed class дословно переводится как «запечатанный класс». В этом классе нужно сразу объявить список классов-наследников, потому что кроме них наследников быть не может. Это похоже на enum, только в разрезе наследования.

Посмотрим, как это выглядит в коде:

```java
sealed class Person permits Student, Teacher, Curator {}
```

Классы Student, Teacher и Curator должны быть в том же пакете или модуле, что и Person. Кроме этого, у них обязательно должен быть один из модификаторов:

- final, если класс запрещён к дальнейшему наследованию:

```java
public final class Student extends Person{}
```

- sealed, если наследование допустимо, но с заранее указанным списком наследников:

```java
public sealed class Teacher extends Person permits MathTeacher, LanguageTeacher {}
```

- non-sealed, когда для класса нужно снять любые ограничения по наследованию:

```java
public non-sealed class Curator extends Person {}
```

У интерфейсов тоже может быть модификатор sealed:

```java
sealed interface Person
   permits Student, Teacher, Curator {
}
```

С учётом record мы можем имплементировать интерфейс и записать класс Student:

```java
public record Student(String name) implements Person{}
```

Здесь record по умолчанию final, поэтому ограничения класса sealed соблюдены.

Запечатанные классы помогают установить ограничение на число наследников, когда их набор определён и его не собираются часто менять. Это похоже на перечисление (enum), но sealed class гибче, потому что одни ветки наследования можно открыть для расширения, а другие ограничить только для использования.
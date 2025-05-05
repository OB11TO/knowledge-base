---
title: Record Class
tags:
  - JavaSE
related_topics: 
created: 2024-08-30 16:46
modified: 2024-09-04T14:39:16+03:00
difficulty: easy
questions: 
notes: 
links: 
---
# Record Class

==Класс record== — одна из самых упоминаемых фич новой версии Java. Она позволяет быстро ==писать иммутабельные POJO-классы==, с ней `не нужно повторять одинаковые методы`: геттеры, toString(), equals() и hashCode().

==POJO (Plain Old Java Object)== — простой` Java-объект, который не унаследован от другого объекта и не реализует интерфейс.`

До того как в Java появился класс record, нам помогал плагин и библиотека [Lombok](https://projectlombok.org/) — она позволяет указать аннотациями, какие методы нужно сгенерировать для класса на этапе компиляции.

Класс record избавляет от бойлерплейтного кода. Для этого не нужны внешние плагины, достаточно встроенных возможностей языка. Чтобы создать класс, нужно указать только два поля — конструктор, геттеры, equals(), hashCode() и toString() уже включены в класс.

Вот как класс выглядит в новой записи:

```java
public record Student(String name, CourseType courseType) {}
```

У класса record есть ==ограничения и особенности:==

- все объявленные `поля получают модификатор final`;
- все `поля` класса `объявляются в заголовке`, дополнительные объявить нельзя:

```java
//ошибка компиляции:
public record Student(String name, CourseType courseType) {
	private int id;
}
```

- `можно объявлять static-поля` класса;
- `класс` record `неявно объявлен как final` , поэтому его нельзя наследовать;
- он `не может быть абстрактным` и наследовать другие классы;
- `можно добавлять свои конструкторы;`
- для конструктора можно использовать проверку аргументов;
- можно п`ереопределить стандартные методы` — геттеры, toString(), equals(), hashCode()
-` можно добавлять статические и нестатические методы.`

У геттеров больше нет приставки get., к методу мы обращаемся по имени переменной:

```java
student.name();
student.courseType();
```

Если нужна валидация данных, конструктор можно расширить или написать свой. При этом стандартный конструктор со всеми параметрами класса остаётся доступным.

Расширенный конструктор:

```java
public record Student(String name, CourseType courseType) {

   public Student {
       if (name == null || name.isBlank()) {
           throw new IllegalArgumentException();
       }
   }
}
```

Кастомный:

```java
public record Student(String name, CourseType courseType) {

   public Student(String name){
       this(name, CourseType.MATH);
   }
}
```
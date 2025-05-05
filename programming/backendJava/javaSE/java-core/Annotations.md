---
title: Annotations
tags:
  - JavaSE
related_topics: 
created: 2024-08-31 12:26
modified: 2025-03-24T13:44:10+03:00
difficulty: medium
questions: 
notes: 
links: 
---

# Annotations

<mark class="hltr-red">Аннотации</mark> — это особая<mark class="hltr-yellow"> форма синтаксических метаданных, которые можно объявить в коде.</mark> Они <mark class="hltr-yellow">используются для анализа кода при компиляции или во время выполнения программы</mark>. Аннотацию можно сравнить с меткой, маркером или указанием для компилятора.

Аннотации применимы не только к методам, ведь они используются в сочетании с пакетами, классами, методами, полями и параметрами.

Удобно рассмотреть случаи применения [аннотаций](https://itsobes.ru/JavaSobes/chem-otlichaetsia-interface-ot-interface) с точки зрения возможных значений их свойства RetentionPolicy

**<mark class="hltr-red">SOURCE</mark>**

– аннотация присутствует <mark class="hltr-green2">только в исходном коде, но не вовлечена в компиляцию</mark>. Можно разделить их на две категории:

Первая – аннотации для программиста, а не для программы. Это всевозможные _маркеры_. Они добавляют аннотируемым элементам некоторую специальную семантику. Более формализованный вариант документации. Примеры – @Immutable и @ThreadSafe из Hibernate.

Вторая категория – _инструкции для инструментов разработки_ . Примеры этой категории, @SuppressWarnings и @Override могут влиять на предупреждения и ошибки компиляции. IntelliJ IDEA умеет понимать @Nullable и @NonNull из Spring Framework, и предупреждать о возможных NullPointerException.

**<mark class="hltr-red">CLASS</mark>**

– самое экзотическое, но при том стандартное значение.<mark class="hltr-green2"> Аннотация попадает в байт-код</mark>

[.class-файла](https://itsobes.ru/JavaSobes/iz-chego-sostoit-class-fail) , <mark class="hltr-green2">но игнорируется</mark> [загрузчиком классов](https://itsobes.ru/JavaSobes/zachem-nuzhen-zagruzchik-klassov) . В результате <mark class="hltr-yellow">такая аннотация недоступна для рефлекшна</mark>. <mark class="hltr-yellow">Используется _для сторонних инструментов_ , обрабатывающих байт-код</mark>, [например](https://stackoverflow.com/a/3849602) для обфускаторов.

<mark class="hltr-red">**RUNTIME**</mark>

– самое ходовое значение. Цель<mark class="hltr-green2"> _снабжается метаинформацией_ , доступной во время выполнения программы.</mark> Сама по себе аннотация всё так же не добавляет нового поведения. Для практической пользы runtime-аннотации в программе должен быть исполнен некоторый код процессинга, который [прочитает](https://stackoverflow.com/a/4296964) метаинформацию инструментами Reflection API.<mark class="hltr-yellow"> Такой механизм широко используется во множестве популярных фреймворков: Spring, Hibernate, Jackson.</mark>

| Аннотация              | Описание                                                                                                  |
| ---------------------- | --------------------------------------------------------------------------------------------------------- |
| `@Override`            | Помечает метод, который переопределяет метод родительского класса или интерфейса.                         |
| `@Deprecated`          | Помечает элемент (метод, класс, поле), который считается устаревшим и не рекомендуется к использованию.   |
| `@SuppressWarnings`    | Подавляет предупреждения компилятора.                                                                     |
| `@FunctionalInterface` | Помечает интерфейс, предназначенный для функционального программирования, имеющий один абстрактный метод. |
| `@SafeVarargs`         | Подавляет предупреждения компилятора для переменных аргументов с параметризованным типом.                 |
| `@Retention`           | Определяет уровень сохранения аннотации (SOURCE, CLASS или RUNTIME).                                      |
| `@Documented`          | Указывает, что аннотация должна быть включена в документацию.                                             |
| `@Inherited`           | Указывает, что аннотация должна быть унаследована подклассами.                                            |
| `@Target`              | Определяет, на какие элементы можно применять аннотацию (TYPE, METHOD, FIELD и т. д.).                    |

### Маркерный интерфейс

До Java 5 для этого использовали интерфейс, не похожий даже сам на себя и не соответствующий своему предназначению. Он был без методов, не нес за собой никакого контракта, просто помечал класс для обособления.

Такой интерфейс назывался маркерным. Из названия следует, что<mark class="hltr-yellow"> его задача — это маркировать классы для JVM, компилятора или какой-либо библиотеки</mark>. До сих пор остались некоторые маркерные интерфейсы, например, _Serializable_. Этот маркер позволяет нам пометить класс, сообщая о том, что его экземпляры можно сериализовать.

<mark class="hltr-red">В случае с интерфейсом мы помечаем класс: даже при неправильном использовании и появлении ошибки она выявится на этапе компиляции и программа не будет запущена.</mark>

<mark class="hltr-red">С аннотациями не все так просто: ошибка выявится уже в рантайме, а это значит, что программа начнет выполнение, но, ожидаемо, не закончит.</mark>

```java
@MyAnnotation
public class MyClass {}

public class MyClass implements MarkerInterface {}
```

### @Target

Аннотация<mark class="hltr-red"> @Target </mark>(актуальна с Java 1.5) <mark class="hltr-yellow">ограничивает возможность применения конфигурируемой аннотации. </mark>Для ограничения до определенного уровня в аннотацию @Target мы должны передать параметр, указывающий, для каких видов она может быть применена.

**Enum [ElementType](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/ElementType.html) - ссылка на документацию**

Если нужна аннотация с несколькими типами, то можно передать несколько параметров в виде массива:

```java
@Target({ ElementType.PARAMETER, ElementType.LOCAL_VARIABLE })
```

|@Target(ElementType.PACKAGE)|для пакетов|
|---|---|
|@Target(ElementType.TYPE)|для классов|
|@Target(ElementType.CONSTRUCTOR)|для конструкторов|
|@Target(ElementType.METHOD)|для методов|
|@Target(ElementType.FIELD)|для атрибутов (переменных) класса|
|@Target(ElementType.PARAMATER)|для параметров метода|
|@Target(ElementType.LOCAL_VARIABLE)|для локальных переменных|
|@Target(ElementType.ANNOTATION_TYPE)|для аннотаций|
|@Target(ElementType.TYPE_PARAMETER)|для параметров типов|
|@Target(ElementType.TYPE_USE)|для использования типов|

### @Retention

Эта<mark class="hltr-yellow"> аннотация укажет, в каком жизненном цикле кода наша аннотация будет доступна.</mark>

Enum [RetentionPolicy](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/RetentionPolicy.html) - ссылка на документацию

| RetentionPolicy.SOURCE  | Аннотации, аннотированные с помощью политики хранения SOURCE, отбрасываются во время выполнения.                                                            |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RetentionPolicy.CLASS   | Аннотации, аннотированные с помощью политики хранения CLASS, записываются в файл .class, но удаляются во время выполнения.                                  |
| RetentionPolicy.RUNTIME | Аннотации, аннотированные с помощью политики хранения RUNTIME, сохраняются во время выполнения и могут быть доступны в нашей программе во время выполнения. |

### Создание собственной аннотации

1 пример

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Info {
   String author() default "Author";
   String version() default "0.0";
}
```

```java
@Info
public class MyClass1 {
   @Info
   public void myClassMethod() {}
}

@Info(version = "2.0")
public class MyClass2 {
   @Info(author = "Anonymous")
   public void myClassMethod() {}
}

@Info(author = "Anonymous", version = "2.0")
public class MyClass3 {
   @Info(author = "Anonymous", version = "4.0")
   public void myClassMethod() {}
}
```

2 пример

```java
//Создание новой аннотации

@Target(value=ElementType.TYPE)
@Retention(value=RetentionPolicy.RUNTIME)
@interface Person
{
 String name() default "";
 int live();
 int strength();
 int magic() default 0;
 int attack() default 0;
 int defense();
}
```

применение к класам

```java
@Person(live=100, strength=10, magic=5, attack=20, defense=20)
class Elf
{
 …
}

@Person(live=1000, strength=150, magic=250, attack=99, defense=99)
class EvilMaster
{
 …
}
```

Как использовать с помощью Reflection API

```java
public boolean fight(Class first, Class second)
{
 if (!first.isAnnotationPresent(Person.class))
  throw new RuntimeException("first param is not game person");
 if (!second.isAnnotationPresent(Person.class))
  throw new RuntimeException("second param is not game person");

 Person firstPerson = (Person) first.getAnnotation(Person.class);
 Person secondPerson = (Person) second.getAnnotation(Person.class);

 int firstAttack = firstPerson.attack() * firstPerson.strength() + firstPerson.magic();
 int firstPower = firstPerson.live() * firstPerson.defense() * firstAttack;

 int secondAttack = secondPerson.attack() * secondPerson.strength() + secondPerson.magic();
 int secondPower = secondPerson.live() * secondPerson.defense() * secondAttack;

 return firstPower > secondPower;
}
```
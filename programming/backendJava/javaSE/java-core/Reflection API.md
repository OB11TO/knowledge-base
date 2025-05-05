---
title: Reflection API
tags:
  - JavaSE
  - WARNING
related_topics: 
created: 2024-08-31 12:45
modified: 2024-09-02T12:35:46+03:00
difficulty: medium
questions: 
notes: 
links: 
---
# Reflection API

Рефлексия в Java — это механизм, который позволяет разработчику вносить изменения и получать информацию о классах, интерфейсах, полях и методах во время выполнения, не зная их имен при этом.

Reflection API также помогает создавать новые экземпляры классов, вызывать методы и получать или устанавливать значения полей.

Минусы использования:

- Нарушения безопасности приложения. С помощью рефлексии мы можем получить доступ к части кода, к которой не должны были (нарушение инкапсуляции).
- Ограничения системы безопасности. Рефлексия требует разрешения времени выполнения, недоступные для систем под управлением менеджера безопасности.
- Низкая производительность. Рефлексия в Java определяет типы динамически, сканируя classpath, чтобы найти класс для загрузки. Это снижает производительность программы.
- Сложность в поддержке. Код, написанный с помощью рефлексии, трудно читать и отлаживать. Он становится менее гибким и его сложнее поддерживать.

### Class

Все операции рефлексии начинаются с объекта java.lang.Class. Для каждого типа объекта создается неизменяемый экземпляр java.lang.Class, который предоставляет методы для получения свойств объекта, создания новых объектов, вызова методов.

### Получение самого экземпляра Class с помощью статического метода Class

- С помощью Class.forName(String name) такой подход возможен в случае, если известно полное имя класса. Тогда можно получить соответствующий класс с помощью статического метода Class.forName(). Этот способ нельзя использовать для примитивных типов.
- `static Class<?>` [`forName](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#forName-java.lang.String->)([String](<https://docs.oracle.com/javase/8/docs/api/java/lang/String.html>) className)`

```java
try {
     Class<?> aClass = Class.forName("com.company.Person");
    } catch (ClassNotFoundException e) {
            e.printStackTrace();
    }
```

### **С помощью .class**

Если тип доступен, но нет экземпляра, то можно получить класс, добавив .class к имени типа. Это самый простой способ получить класс для примитивного типа.

```java
Class aClass = Person.class;
```

### C помощью .getClass()

Если экземпляр объекта доступен, то самый простой способ получить его класс — вызвать object.getClass().

```java
Person person = new Person();
Class aClass = person.getClass();
```

### Методы класса Class

- `<U> [Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<? extends U>` [`asSubclass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#asSubclass-java.lang.Class->)([Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<U> clazz)` используется для проверки того, можно ли привести данный класс к заданному классу-предку. Если приведение возможно, метод возвращает объект типа `Class` для подкласса, в противном случае генерируется исключение `ClassCastException`.

```java
class Animal {
    public void makeSound() {
        System.out.println("Animal is making a sound");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog is barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();
        Class<? extends Animal> animalClass = animal.getClass().asSubclass(Animal.class);
        animalClass.cast(animal).makeSound();
    }
}
```

- [`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) [`cast](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#cast-java.lang.Object->)([Object](<https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html>) obj)` позволяет выполнить явное приведение объекта `obj` к типу, представленному данным объектом `Class`. Если `obj` не может быть приведен к заданному типу, метод генерирует исключение `ClassCastException`.

```java
class Animal {
    public void makeSound() {
        System.out.println("Animal is making a sound");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog is barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();
        Class<Animal> animalClass = Animal.class;
        Dog dog = animalClass.cast(animal); // явное приведение объекта animal к типу Dog
        dog.makeSound(); // вызов переопределенного метода у объекта типа Dog
    }
}
```

- `boolean` [`desiredAssertionStatus](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#desiredAssertionStatus-->)()` используется для получения статуса включения или отключения утверждений во время выполнения программы. Если утверждения включены, метод возвращает значение `true`, в противном случае - `false`.

```java
public class Main {
    public static void main(String[] args) {
        boolean assertionsEnabled = Main.class.desiredAssertionStatus();
        System.out.println("Assertions enabled: " + assertionsEnabled);
        assert false : "This assertion will fail";
    }
}
```

- [`AnnotatedType](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedType.html>)[]` [`getAnnotatedInterfaces](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getAnnotatedInterfaces-->)()` возвращает массив всех интерфейсов, реализованных данным классом, в виде объектов `AnnotatedType`. Каждый элемент массива представляет один интерфейс, реализованный данным классом, и включает в себя дополнительную информацию о типе, такую как аннотации, примененные к типу.

```java
public class App implements Comparable<App>, Serializable,Cloneable {
    public static void main(String[] args) {
        Class<?> exampleClass = App.class;
        AnnotatedType[] annotatedInterfaces = exampleClass.getAnnotatedInterfaces();

        for (AnnotatedType annotatedInterface : annotatedInterfaces) {
            System.out.println(annotatedInterface.getType());
        }
    }

    @Override
    public int compareTo(App o) {
        return 0;
    }
}

ВЫВОД: 
java.lang.Comparable<org.example.App>
interface java.io.Serializable
interface java.lang.Cloneable
```

- [`AnnotatedType`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedType.html)

```java
Class<?> exampleClass = String.class;
        AnnotatedType annotatedSuperclass = exampleClass.getAnnotatedSuperclass();
        System.out.println(annotatedSuperclass.getType());
```

- [`Annotation](<https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Annotation.html>)[]` [`getAnnotations](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getAnnotations-->)()` возвращает массив объектов `Annotation`, представляющих все аннотации, примененные к данному классу (включая аннотации унаследованные от суперклассов).

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.reflect.AnnotatedType;
import java.lang.reflect.Field;

@Retention(RetentionPolicy.RUNTIME)
@interface ExampleAnnotation {
    String value();
}

@ExampleAnnotation("Example")
public class Example {
    private int field;

    public static void main(String[] args) {
        Class<?> exampleClass = Example.class;
        Annotation[] annotations = exampleClass.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);
        }

        Field[] fields = exampleClass.getDeclaredFields();
        for (Field field : fields) {
            Annotation[] fieldAnnotations = field.getAnnotations();
            for (Annotation annotation : fieldAnnotations) {
                System.out.println(annotation);
            }
        }
    }

    @ExampleAnnotation("Field")
    public int getField() {
        return field;
    }
}
```

- `<A extends [Annotation](<https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Annotation.html>)>` `A[]` `A[][getAnnotationsByType](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getAnnotationsByType-java.lang.Class->)([Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<A> annotationClass)` возвращает все аннотации, которые применены к данному элементу программы и имеют тип `annotationClass`. Метод возвращает массив типа `A[]`, где `A` - это тип запрашиваемой аннотации.

```java
import java.lang.annotation.Annotation;
import java.lang.reflect.Field;

public class App implements Comparable<App>, java.io.Serializable, Cloneable {
    @Override
    public int compareTo(App o) {
        return 0;
    }

    @SuppressWarnings("unused")
    @CustomAnnotation
    private int privateField;

    @CustomAnnotation
    public void customAnnotatedMethod() {}

    public static void main(String[] args) {
        Class<?> appClass = App.class;

        // Получение всех аннотаций типа CustomAnnotation, примененных к классу App
        CustomAnnotation[] annotations = appClass.getAnnotationsByType(CustomAnnotation.class);
        for (CustomAnnotation annotation : annotations) {
            System.out.println(annotation);
        }

        Field[] fields = appClass.getDeclaredFields();
        for (Field field : fields) {
            // Получение всех аннотаций типа CustomAnnotation, примененных к каждому полю класса App
            CustomAnnotation[] fieldAnnotations = field.getAnnotationsByType(CustomAnnotation.class);
            for (CustomAnnotation annotation : fieldAnnotations) {
                System.out.println(annotation);
            }
        }
    }
}

// Объявление пользовательской аннотации CustomAnnotation
@interface CustomAnnotation {}
```

- [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) [`getCanonicalName](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getCanonicalName-->)()` возвращает каноническое имя класса в виде строки. Каноническое имя класса - это полное имя класса, включая пакет, но без символа `$` для внутренних классов.

```java
public class MyClass {}

public class Main {
  public static void main(String[] args) {
    Class myClass = MyClass.class;
    System.out.println(myClass.getCanonicalName()); // Output: "MyClass"
  }
}
```

- [`Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>[]` [`getClasses](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getClasses-->)()` возвращает массив объектов класса `Class`, представляющих все публичные классы и интерфейсы, объявленные в этом классе или интерфейсе.Результатом выполнения этого кода будет вывод на консоль имени класса `MyInnerClass`, так как это единственный публичный класс в `MyClass`. Если бы у нас было несколько публичных классов в `MyClass`, то все они были бы выведены в этом цикле.

```java
public class MyClass {
    public class MyInnerClass {}
}
Class<?>[] classes = MyClass.class.getClasses();
for (Class<?> c : classes) {
    System.out.println(c.getName());
}
```

- [`ClassLoader`](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassLoader.html) [`getClassLoader](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getClassLoader-->)()` возвращает загрузчик класса, который загрузил этот класс. Загрузчик класса является объектом, который отвечает за загрузку классов в Java-приложение.

```java
Class<?> myClass = MyClass.class;
ClassLoader classLoader = myClass.getClassLoader();
System.out.println(classLoader.toString());

//jdk.internal.loader.ClassLoaders$AppClassLoader@251a69d7
```

- [`Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>` [`getComponentType](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getComponentType-->)()` возвращает класс, представляющий тип элемента массива, если данный класс является массивом. Если данный класс не является массивом, то метод возвращает `null`.

```java
Class<?> intArrayClass = int[].class;
        Class<?> componentType = intArrayClass.getComponentType();
        Class<?> notArray = App.class.getComponentType();
        System.out.println(notArray.getName()); //Exception in thread "main" java.lang.NullPointerException:
        // Cannot invoke "java.lang.Class.getName()" because "some" is null
        System.out.println(componentType.getName()); // Выводит "int"
```

- [`Constructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html>)<[T](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)>` [`getConstructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructor-java.lang.Class>...-)([Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)\<?>... parameterTypes)` используется для получения конструктора класса с заданными параметрами.

Исключения:

- `NoSuchMethodException` - если не удается найти указанный конструктор.
- `SecurityException` - если вызывающий метод не имеет доступа к конструктору.

```java
public class MyClass {
    public MyClass(String str, int i) {
        // constructor implementation
    }
}

public class Main {
    public static void main(String[] args) throws NoSuchMethodException {
        Class<MyClass> myClass = MyClass.class;
        Constructor<MyClass> constructor = myClass.getConstructor(String.class, int.class);
        System.out.println(constructor);
    }
}
```

- [`Constructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html>)<?>[]` [`getConstructors](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getConstructors-->)()` возвращает массив всех `public` конструкторов, объявленных в данном классе или интерфейсе.

```java
import java.lang.reflect.Constructor;

public class MyClass {
    private String name;
    private int age;

    public MyClass(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public MyClass() {
        this("default", 0);
    }

    public static void main(String[] args) {
        Class<?> myClass = MyClass.class;
        Constructor<?>[] constructors = myClass.getConstructors();

        for (Constructor<?> constructor : constructors) {
            System.out.println(constructor);
        }
    }
}
```

- `<A extends [Annotation](<https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Annotation.html>)>` `A` [`getDeclaredAnnotation](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredAnnotation-java.lang.Class->)([Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<A> annotationClass)` возвращает аннотацию указанного типа, объявленную в данном классе или интерфейсе. Метод не рекурсивно просматривает родительские классы или интерфейсы. Если аннотация данного типа не объявлена, метод вернет `null`. Если аннотация была объявлена, но не сохранилась в `.class` файле, метод также вернет `null`.

```java
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String value();
}

public class MyClass {
    @MyAnnotation("test")
    public void myMethod() {
        // ...
    }
}

public static void main(String[] args) {
    Class<MyClass> clazz = MyClass.class;
    MyAnnotation annotation = clazz.getDeclaredAnnotation(MyAnnotation.class);
    System.out.println(annotation.value()); // Output: "test"
}
```

- [`Annotation](<https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Annotation.html>)[]` [`getDeclaredAnnotations](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredAnnotations-->)()`возвращает все объявленные в классе аннотации, включая как аннотации типов, так и аннотации наследуемых элементов.

```java
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface MyAnnotation {
    String value();
}

@MyAnnotation("test")
public class MyClass {
    public static void main(String[] args) {
        Class<?> myClass = MyClass.class;
        Annotation[] annotations = myClass.getDeclaredAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation.toString());
        }
    }
}

ВЫВОД: @MyAnnotation(value=test)
```

- `<A extends [Annotation](<https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Annotation.html>)>` `A[]` [`getDeclaredAnnotationsByType](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredAnnotationsByType-java.lang.Class->)([Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<A> annotationClass)` позволяет получить все аннотации заданного типа, объявленные в данном классе или интерфейсе, включая прямые и косвенные аннотации.
- Метод вернет массив аннотаций, объявленных в `MyClass`, и имеющих тип `@MyAnnotation`. Если в `MyClass` не объявлены аннотации этого типа, то метод вернет пустой массив.

```java
@MyAnnotation
public class MyClass {
  // some code here
}
MyClass myClass = new MyClass();
Annotation[] annotations = myClass.getDeclaredAnnotationsByType(MyAnnotation.class);
```

- [`Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>[]` [`getDeclaredClasses](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredClasses-->)()` возвращает массив объектов класса `Class`, представляющих объявленные внутри данного класса вложенные классы, интерфейсы, перечисления и аннотации. Приватные в том числе

```java
public class OuterClass {
    public static class NestedClass {
        // ...
    }
    private class InnerClass {
        // ...
    }
    public interface NestedInterface {
        // ...
    }
}

public class Main {
    public static void main(String[] args) {
        Class<?>[] declaredClasses = OuterClass.class.getDeclaredClasses();
        for (Class<?> declaredClass : declaredClasses) {
            System.out.println(declaredClass.getName());
        }
    }
}

ВЫВОД:
OuterClass$NestedClass
OuterClass$InnerClass
OuterClass$NestedInterface
```

- [`Constructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html>)<[T](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)>` [`getDeclaredConstructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructor-java.lang.Class>...-)([Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)\<?>... parameterTypes)` возвращает объект `Constructor` для определенного конструктора этого класса с заданными параметрами. Если такой конструктор существует, он будет выведен в консоль. Если же такого конструктора нет, будет выброшено исключение `NoSuchMethodException`.

```java
import java.lang.reflect.Constructor;

public class MyClass {
    public static void main(String[] args) throws Exception {
        Class<?> clazz = MyObject.class;
        Constructor<?> constructor = clazz.getDeclaredConstructor(int.class, String.class);
        System.out.println(constructor);
    }
}

class MyObject {
    public MyObject(int i, String s) {}
}
```

- [`Constructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html>)<?>[]` [`getDeclaredConstructors](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredConstructors-->)()` возвращает все конструкторы данного класса, в том числе конструкторы, объявленные как `private`.

```java
import java.lang.reflect.Constructor;

public class MyClass {
    private int myField;

    public MyClass(int myField) {
        this.myField = myField;
    }

    private MyClass() {
        // Private constructor
    }

    public static void main(String[] args) {
        Class<?> myClass = MyClass.class;
        Constructor<?>[] constructors = myClass.getDeclaredConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println(constructor.getName());
        }
    }
}

вывод:
MyClass
MyClass
```

- [`Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html) [`getDeclaredField](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredField-java.lang.String->)([String](<https://docs.oracle.com/javase/8/docs/api/java/lang/String.html>) name)` возвращает объект типа `Field`, представляющий поле с заданным именем, объявленное в этом классе. Если поле не объявлено в этом классе, то возвращается `null`.

```java
public class MyClass {
    private int myPrivateField;
    public String myPublicField;
}

public class Main {
    public static void main(String[] args) throws Exception {
        Class<MyClass> clazz = MyClass.class;
        Field privateField = clazz.getDeclaredField("myPrivateField");
        System.out.println(privateField.getName());
        Field publicField = clazz.getDeclaredField("myPublicField");
        System.out.println(publicField.getName());
    }
}

ВЫВОД:
myPrivateField
myPublicField
```

- [`Field](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html>)[]` [`getDeclaredFields](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredFields-->)()` возвращает массив объектов типа `Field`, представляющих все поля, объявленные в этом классе или интерфейсе. Этот метод возвращает только те поля, к которым можно получить доступ из кода этого класса, и не включает поля родительских классов или интерфейсов.

```java
import java.lang.reflect.Field;

public class MyClass {
  private int myPrivateField;
  public String myPublicField;
  protected boolean myProtectedField;
  
  public static void main(String[] args) {
    Class<?> clazz = MyClass.class;
    Field[] fields = clazz.getDeclaredFields();
    for (Field field : fields) {
      System.out.println(field.getName() + " (" + field.getType().getSimpleName() + ")");
    }
  }
}

ВЫВОД: 
myPrivateField (int)
myPublicField (String)
myProtectedField (boolean)
```

- [`Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html) [`getDeclaredMethod](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethod-java.lang.String-java.lang.Class>...-)([String](<https://docs.oracle.com/javase/8/docs/api/java/lang/String.html>) name, [Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>... parameterTypes)` извлекает объект `Method`, который соответствует заданному имени и параметрам метода. Этот метод возвращает только те методы, которые объявлены в данном классе, и не включает методы из его суперклассов или реализуемых интерфейсов.

```java
import java.lang.reflect.Method;

public class Example {
    public void foo(String s, int i) {
        // implementation
    }
    
    public static void main(String[] args) throws NoSuchMethodException {
        Class<?> cls = Example.class;
        
        Method method = cls.getDeclaredMethod("foo", String.class, int.class);
        
        System.out.println(method);
    }
}

ВЫВОД: public void Example.foo(java.lang.String,int)
```

- [`Method](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html>)[]` [`getDeclaredMethods](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaredMethods-->)()` возвращает все методы, объявленные в этом классе, включая приватные, защищенные и публичные методы, но не включая методы, унаследованные от родительских классов или интерфейсов. Исключения: `SecurityException` - если отсутствует необходимое разрешение.

```java
import java.lang.reflect.*;

public class Example {
  public static void main(String[] args) {
    // Получение объекта Class для класса MyClass
    Class myClass = MyClass.class;
    // Получение всех объявленных методов в MyClass
    Method[] methods = myClass.getDeclaredMethods();
    // Вывод имени каждого метода
    for (Method method : methods) {
      System.out.println(method.getName());
    }
  }
}

class MyClass {
  public void publicMethod() {}
  protected void protectedMethod() {}
  private void privateMethod() {}
}

ВЫВОД: 
publicMethod
protectedMethod
privateMethod
```

- [`Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>` [`getDeclaringClass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getDeclaringClass-->)()` возвращает класс, в котором был объявлен класс, представленный текущим объектом `Class`. Если класс был объявлен в верхнем уровне, то возвращается `null`.

```java
public class MyClass {
    private static class MyInnerClass {
    }
}

public class Main {
    public static void main(String[] args) {
        Class<?> innerClass = MyClass.MyInnerClass.class;
        Class<?> outerClass = innerClass.getDeclaringClass();
        System.out.println(outerClass.getName()); // Output: "MyClass"
    }
}
```

- [`Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>` [`getEnclosingClass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnclosingClass-->)()` возвращает класс, который объемлет данный класс. Если данный класс находится на вершине иерархии, то метод возвращает `null`.

```java
public class Outer {
    public class Inner {}

    public static void main(String[] args) {
        Class<?> innerClass = Inner.class;
        Class<?> enclosingClass = innerClass.getEnclosingClass();

        System.out.println(enclosingClass.getName()); // Output: "Outer"
    }
}
```

- [`Constructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Constructor.html>)<?>` [`getEnclosingConstructor](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnclosingConstructor-->)()` возвращает конструктор, в котором был объявлен данный класс. Если класс был объявлен не в конструкторе, то метод возвращает `null`.

```java
public class OuterClass {
    public static class InnerClass {
        public InnerClass() {}
    }
    
    public static void main(String[] args) {
        Class<?> innerClass = InnerClass.class;
        Class<?> outerClass = innerClass.getEnclosingClass();
        
        Constructor<?> enclosingConstructor = innerClass.getEnclosingConstructor();
        
        System.out.println(enclosingConstructor);
 // Output: public OuterClass$InnerClass()
    }
}
```

- [`Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html) [`getEnclosingMethod](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnclosingMethod-->)()` возвращает объект типа `Method`, представляющий метод, в котором определен вложенный класс, либо `null`, если класс не является членом другого класса.

```java
public class Outer {
    class Inner {
        public void printEnclosingMethod() {
            Method enclosingMethod = getClass().getEnclosingMethod();
            if (enclosingMethod == null) {
                System.out.println("Class " + getClass().getSimpleName() + " is not an inner class");
            } else {
                System.out.println("Enclosing method of " + getClass().getSimpleName() + " is " + enclosingMethod.getName());
            //ВЫВОД: Enclosing method of Inner is doSomething
						}
        }
    }

    public void doSomething() {
        Inner inner = new Inner();
        inner.printEnclosingMethod();
    }
}

// Создаем объект внешнего класса и вызываем его метод
Outer outer = new Outer();
outer.doSomething();
```

- [`T](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)[]` [`getEnumConstants](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getEnumConstants-->)()` возвращает массив констант перечисления (enum), представленного данным объектом `Class`, или `null`, если данный класс не является перечислением.

```java
public enum Month {
    JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC
}

public class Example {
    public static void main(String[] args) {
        Class<?> monthClass = Month.class;
        Object[] constants = monthClass.getEnumConstants();
        for (Object constant : constants) {
            System.out.println(constant);
        }
    }
}

// Вывод:
// JAN
// FEB
// MAR
// APR
// MAY
// JUN
// JUL
// AUG
// SEP
// OCT
// NOV
// DEC
```

- [`Field`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html) [`getField](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getField-java.lang.String->)([String](<https://docs.oracle.com/javase/8/docs/api/java/lang/String.html>) name)` возвращает открытое поле класса с указанным именем, или выбрасывает исключение `NoSuchFieldException`, если такое поле не найдено.

```java
import java.lang.reflect.Field;

public class Example {
    public int publicField;
    private int privateField;
    public static void main(String[] args) throws NoSuchFieldException {
        Class<?> exampleClass = Example.class;

        // получение открытого поля класса Example по имени
        Field publicField = exampleClass.getField("publicField");
        System.out.println(publicField.getName()); // Output: publicField

        // попытка получить приватное поле класса Example по имени - выброс исключения NoSuchFieldException
        Field privateField = exampleClass.getField("privateField");
/*
publicField

Exception in thread "main" java.lang.NoSuchFieldException: privateField
at java.base/java.lang.Class.getField(Class.java:1993)
at Example.main(Example.java:13)
*/
    }
}
```

- [`Field](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Field.html>)[]` [`getFields](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getFields-->)()`

```java
import java.lang.reflect.Field;

public class MyClass {
    public int publicField1;
    public String publicField2;
    private double privateField;

    public static void main(String[] args) {
        Field[] fields = MyClass.class.getFields();
        for (Field field : fields) {
            System.out.println(field.getName());
/*
publicField1
publicField2
*/
        }
    }
}
```

- [`Type](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Type.html>)[]` [`getGenericInterfaces](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getGenericInterfaces-->)()`

```java
public interface MyInterface<T> {}

public class MyClass implements MyInterface<String> {}

public class Main {
  public static void main(String[] args) {
    Class<?> myClass = MyClass.class;
    Type[] genericInterfaces = myClass.getGenericInterfaces();

    for (Type genericInterface : genericInterfaces) {
      System.out.println(genericInterface.getTypeName());
//MyInterface<java.lang.String>
    }
  }
}
```

- [`Type`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Type.html) [`getGenericSuperclass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getGenericSuperclass-->)()` возвращает суперкласс данного класса в виде объекта `Type`, который представляет его в обобщенной форме.

```java
import java.lang.reflect.*;

class MyClass<T> {}

public class Main {
  public static void main(String[] args) {
    Type type = MyClass.class.getGenericSuperclass();
    System.out.println(type); //class java.lang.Object
  }
}
```

- [`Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>[]` [`getInterfaces](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getInterfaces-->)()` возвращает массив интерфейсов, которые реализует или расширяет данный класс.

```java
public class MyInterface {}
public interface MyOtherInterface {}

public class MyClass implements MyInterface, MyOtherInterface {}

public class Main {
    public static void main(String[] args) {
        Class<?> myClass = MyClass.class;
        Class<?>[] interfaces = myClass.getInterfaces();
        for (Class<?> i : interfaces) {
            System.out.println(i.getName());
        }
    }
}

// Output:
// MyInterface
// MyOtherInterface
```

- [`Method`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html) [`getMethod](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethod-java.lang.String-java.lang.Class>...-)([String](<https://docs.oracle.com/javase/8/docs/api/java/lang/String.html>) name, [Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?>... parameterTypes)` который используется для получения открытого метода класса по его имени и типам параметров.Если метод с заданным именем и типами параметров не найден в классе, то возвращается `null`.

```java
public class MyClass {
    public void myMethod(int num, String str) {
        // code
    }
}

public class Main {
    public static void main(String[] args) throws NoSuchMethodException {
        Class<?> myClass = MyClass.class;
        Method myMethod = myClass.getMethod("myMethod", int.class, String.class);
        System.out.println(myMethod);
    }
}
```

- [`Method](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Method.html>)[]` [`getMethods](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getMethods-->)()` возвращает массив объектов `Method`, представляющих публичные методы, доступные в данном классе (включая методы, унаследованные от родительских классов и интерфейсов).

```java
import java.lang.reflect.*;

class MyClass {
    public void doSomething() {
        System.out.println("Doing something...");
    }

    public void doSomethingElse() {
        System.out.println("Doing something else...");
    }
}

public class Main {
    public static void main(String[] args) {
        Class<?> myClass = MyClass.class;
        Method[] methods = myClass.getMethods();
        for (Method method : methods) {
            System.out.println(method.getName());
        }
    }
}

doSomething
doSomethingElse
wait
wait
wait
equals
toString
hashCode
getClass
notify
notifyAll
```

- `int` [`getModifiers](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getModifiers-->)()` возвращает целочисленное значение, которое представляет модификаторы доступа и другие модификаторы данного класса.

```java
public class MyClass {
    public static void main(String[] args) {
        Class<MyClass> clazz = MyClass.class;
        int modifiers = clazz.getModifiers();
        System.out.println(Modifier.isPublic(modifiers));
 // выводит true,  класс имеет модификатор доступа public
        System.out.println(Modifier.isAbstract(modifiers));
 // выводит false,  класс не является абстрактным
    }
}
```

- [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) [`getName](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getName-->)()` возвращает полное имя этого класса в виде строки. Полное имя включает название пакета и имя класса. Например, если класс находится в пакете `com.example` и имеет имя `MyClass`, то его полное имя будет `com.example.MyClass`.

```java
public class MyClass {
    public static void main(String[] args) {
        Class<MyClass> clazz = MyClass.class;
        System.out.println(clazz.getName()); // выводит полное имя класса
    }
}
```

- [`Package`](https://docs.oracle.com/javase/8/docs/api/java/lang/Package.html) [`getPackage](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getPackage-->)()` возвращает пакет этого класса в виде объекта `Package`. Если класс не находится в пакете, то возвращается `null`.

```java
public class MyClass {
    public static void main(String[] args) {
        Class<MyClass> clazz = MyClass.class;
        Package pkg = clazz.getPackage();
        System.out.println(pkg.getName()); // выводит название пакета
    }
}
```

- [`ProtectionDomain`](https://docs.oracle.com/javase/8/docs/api/java/security/ProtectionDomain.html) [`getProtectionDomain](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getProtectionDomain-->)()`который возвращает объект `ProtectionDomain`, представляющий защитную область данного класса. Защитная область определяет политику безопасности, которая будет применяться к классу, включая доступ к файлам, сетевые соединения и другие ресурсы.

```java
public class Example {
    public static void main(String[] args) {
        // Получить защитную область для класса Example
        ProtectionDomain protectionDomain = Example.class.getProtectionDomain();

        // Вывести информацию о защитной области
        System.out.println(protectionDomain);
    }
}

ProtectionDomain  (file:/home/vadim/IdeaProjects/CollectorsMethods/target/classes/ <no signer certificates>)
 jdk.internal.loader.ClassLoaders$AppClassLoader@251a69d7
 <no principals>
 java.security.Permissions@2ff4acd0 (
 ("java.lang.RuntimePermission" "exitVM")
 ("java.io.FilePermission" "/home/vadim/IdeaProjects/CollectorsMethods/target/classes/-" "read")
)
```

- [`URL`](https://docs.oracle.com/javase/8/docs/api/java/net/URL.html) [`getResource](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getResource-java.lang.String->)([String](<https://docs.oracle.com/javase/8/docs/api/java/lang/String.html>) name)` который используется для получения объекта `URL`, который представляет местоположение ресурса с заданным именем.

```java
public class Example {
    public static void main(String[] args) {
        // Get the URL of a resource in the same package as this class
        URL url = Example.class.getResource("resource.txt");

        // Print the URL
        System.out.println(url);
    }
}
```

- [`InputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html) [`getResourceAsStream](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getResourceAsStream-java.lang.String->)([String](<https://docs.oracle.com/javase/8/docs/api/java/lang/String.html>) name)` возвращает `InputStream`, связанный с указанным ресурсом.Ресурсы, связанные с классом, могут находиться в различных местах, таких как файлы в системе файлов, JAR-файлы или директории в classpath. Поиск ресурсов происходит в соответствии с правилами, определенными в спецификации `ClassLoader`.

```java
import java.io.InputStream;
import java.util.Scanner;

public class MyClass {
    public static void main(String[] args) {
        Class<?> myClass = MyClass.class;
        InputStream inputStream = myClass.getResourceAsStream("/example.txt");
        Scanner scanner = new Scanner(inputStream);
        while (scanner.hasNextLine()) {
            String line = scanner.nextLine();
            System.out.println(line);
        }
    }
}
```

- [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) [`getSimpleName](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getSimpleName-->)()` возвращает простое (неполное) имя класса без учета его пакета. Если класс не имеет простого имени (например, анонимный класс), то метод вернет пустую строку.

```java
public class MyClass {}

public class Main {
  public static void main(String[] args) {
    Class<?> myClass = MyClass.class;
    System.out.println(myClass.getSimpleName()); // Output: "MyClass"
  }
}
```

- [`Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<? super [T](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)>` [`getSuperclass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getSuperclass-->)()` возвращает класс-родительский класс для данного класса. В Java каждый класс кроме `Object` имеет свой родительский класс, поэтому если класс не наследуется от другого класса, то метод вернет значение `null`.

```java
public class Animal {}
public class Mammal extends Animal {}

public class Main {
  public static void main(String[] args) {
    Class<?> mammalClass = Mammal.class;
    Class<?> animalClass = mammalClass.getSuperclass();
    System.out.println(animalClass.getName()); // Output: "Animal"
  }
}
```

- [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) [`getTypeName](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getTypeName-->)()` возвращает имя типа, представленного данным объектом `Class`. Имя типа включает имя самого типа и, если тип является обобщенным, имена типов аргументов.

```java
public class Example<T> {
    public static void main(String[] args) {
        Example<String> example = new Example<>();
        System.out.println(example.getClass().getTypeName()); // Example<java.lang.String>
    }
}
```

- [`TypeVariable](<https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/TypeVariable.html>)<[Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<[T](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)>>[]` [`getTypeParameters](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#getTypeParameters-->)()` возвращает массив типов переменных, объявленных для данного класса, в виде объектов класса `TypeVariable<Class\<T>>`. Каждый элемент массива представляет одну переменную типа, объявленную для данного класса.

```java
Class<?> exampleClass = Example.class;
        TypeVariable<?>[] typeParameters = exampleClass.getTypeParameters();
        for (TypeVariable<?> typeParameter : typeParameters) {
            System.out.println(typeParameter.getName()); // T,E
        }
    }
    public class Example<T,E> extends ArrayList<java.lang.Integer>{
    }
```

- `boolean` [`isAnnotation](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isAnnotation-->)()` [`isAnonymousClass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isAnonymousClass-->)()` проверяет, является ли данный класс анонимным классом.

```java
public class Example {
    public static void main(String[] args) {
        Class<?> annotationClass = Deprecated.class;
        Class<?> anonymousClass = new Object(){}.getClass();
       
        System.out.println(anonymousClass.isAnonymousClass()); // true
    }
}
```

- `boolean` [`isArray](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isArray-->)()` возвращает `true`, если данный класс представляет массив, и `false` в противном случае.

```java
public class Example {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3};
        Class<?> numbersClass = numbers.getClass();
        boolean isArray = numbersClass.isArray();
        System.out.println(isArray); // true

				List<Integer> numbers = new ArrayList<>();
		    Class<?> clazz = numbers.getClass();
        System.out.println(clazz.isArray()); // false
    }
}
```

- `boolean` [`isAssignableFrom](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isAssignableFrom-java.lang.Class->)([Class](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html>)<?> cls)` пределяет, можно ли присвоить переменной данного класса объект указанного класса `cls`.

```java
public class Example {
    public static void main(String[] args) {
        Class<?> superClass = Object.class;
        Class<?> subClass = String.class;
        boolean isAssignableFrom = superClass.isAssignableFrom(subClass);
        System.out.println(isAssignableFrom); // true
    }
}
```

- `boolean` [`isEnum](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isEnum-->)()` возвращает `true`, если данный класс является перечислением (enum), и `false` в противном случае.

```java
public enum Color {
    RED, GREEN, BLUE;
}

public class Example {
    public static void main(String[] args) {
        Class<?> colorClass = Color.class;
        boolean isEnum = colorClass.isEnum();
        System.out.println(isEnum); // true
    }
}
```

- `boolean` [`isInstance](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isInstance-java.lang.Object->)([Object](<https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html>) obj)` проверяет, является ли указанный объект экземпляром данного класса или его подкласса.

```java
public class Example {
    public static void main(String[] args) {
        Object str = "hello";
        Class<?> stringClass = String.class;
        boolean isInstance = stringClass.isInstance(str);
        System.out.println(isInstance); // true
    }
}
```

- `boolean` [`isInterface](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isInterface-->)()` возвращает `true`, если данный класс является интерфейсом, и `false` в противном случае.

```java
public interface MyInterface {
    void myMethod();
}

public class Example {
    public static void main(String[] args) {
        Class<?> interfaceClass = MyInterface.class;
        boolean isInterface = interfaceClass.isInterface();
        System.out.println(isInterface); // true
    }
}
```

- `boolean` [`isLocalClass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isLocalClass-->)()` возвращает `true`, если данный класс является локальным классом (вложенным классом, определенным внутри другого класса или метода), и `false` в противном случае.

```java
public class Example {
    public void myMethod() {
        class LocalClass {
            // ...
        }
        Class<?> localClass = LocalClass.class;
        boolean isLocalClass = localClass.isLocalClass();
        System.out.println(isLocalClass); // true
    }
}
```

- `boolean` [`isMemberClass](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isMemberClass-->)()` возвращает `true`, если данный класс является внутренним классом (членом другого класса), и `false` в противном случае.

```java
public class OuterClass {
    public class InnerClass {
        // ...
    }
}

public class Example {
    public static void main(String[] args) {
        Class<?> innerClass = OuterClass.InnerClass.class;
        boolean isMemberClass = innerClass.isMemberClass();
        System.out.println(isMemberClass); // true
    }
}
```

- `boolean` [`isPrimitive](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isPrimitive-->)()`

```java
public class Example {
    public static void main(String[] args) {
        Class<?> intClass = int.class;
        boolean isPrimitive = intClass.isPrimitive();
        System.out.println(isPrimitive); // true
    }
}
```

- `boolean` [`isSynthetic](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#isSynthetic-->)()` возвращает `true`, если данный класс был создан компилятором в процессе компиляции и не существовал в исходном коде, и `false` в противном случае.

```java
public class Example {
    public static void main(String[] args) {
        class LocalClass {
            // ...
        }
        Class<?> localClass = LocalClass.class;
        boolean isSynthetic = localClass.isSynthetic();
        System.out.println(isSynthetic); // true or false, depending on the compiler and context
    }
}
```

- [`T`](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html) [`newInstance](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#newInstance-->)()` создает новый объект, представляющий данный класс. Для этого класс **должен иметь публичный конструктор без параметров.**

```java
public class Example {
    public static void main(String[] args) throws IllegalAccessException, InstantiationException {
        Class<?> stringClass = String.class;
        String newString = (String) stringClass.newInstance();
        System.out.println(newString); // an empty string
    }
}
```

- [`String`](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html) [`toGenericString](<https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html#toGenericString-->)()` возвращает строковое представление объявления данного класса, включая его модификаторы, имя, типы параметров (для параметризованных типов), имя суперкласса и список интерфейсов.

```java
public class Example {
    public static void main(String[] args) {
        Class<?> stringClass = String.class;
        String genericString = stringClass.toGenericString();
        System.out.println(genericString);
 // "public final class java.lang.String
 //implements java.io.Serializable,
 //java.lang.Comparable<java.lang.String
 //java.lang.CharSequence"
    }
}
```


### Создание объекта с Reflection API

[https://javarush.com/quests/lectures/jru.module2.lecture34](https://javarush.com/quests/lectures/jru.module2.lecture34)

**Преимущества создания через Constructor.newInstance()**

|Class.newInstance()|Constructor.newInstance()|
|---|---|
|Может вызывать только конструктор no-arg.|Может вызывать любой конструктор независимо от количества параметров.|
|Требует, чтобы конструктор был виден.|Также может вызывать приватные конструкторы при определенных обстоятельствах.|
|Выдает любое исключение (проверяемое или нет), которое задекларировано конструктором.|Всегда обертывает выданное исключение с помощью InvocationTargetException.|

```java
package org.example;

public class Person {
    public int id;
    private String name;
    private int age;
    private Person(){
    }

    private Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    private void doBadThings(){
        System.out.println(this +  " говорит F*ck");
    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                ", name='" + name + '\\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
package org.example;

import java.lang.reflect.*;

public class App  {
    public static void main(String[] args) throws InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
    Class<? extends Person> clazz = Person.class;
        Field [] fields = clazz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println(field);
        }
//        Person person = clazz.newInstance(); // бросает исключение так, как пустой конструктор private
        Constructor<? extends Person> personConstructor = clazz.getDeclaredConstructor(String.class, int.class);
//        Person person = personConstructor.newInstance("Vadim", 28); // выскакивает ошибка, так как не доступа к созданию приватного конструктора
        personConstructor.setAccessible(true);
        Person person = personConstructor.newInstance("Vadim", 28); // а тут нет
        person.id = 1;
        System.out.println(person); // Person{id=1, name='Vadim', age=28}
        person. // метод doBadThings невидно, так как доступа к нему нет(private)
        Method methodThrowsException = person.getClass().getMethod("doBadThings"); //ошибка так метод private
        Method method2 = person.getClass().getDeclaredMethod("doBadThings"); // тут ошибка, так как не разрешили доступ
        method2.setAccessible(true);
        method2.invoke(person); //Person{id=1, name='Vadim', age=28} говорит F*ck

        }
    }
```

- `Побитовая операция в маске`
![[Pasted image 20240902123533.png]]
![[Pasted image 20240902123552.png]]
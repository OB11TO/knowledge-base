---
title: Nested class
tags:
  - JavaSE
  - InnerClass
related_topics: 
created: 2024-08-30 14:02
modified: 2025-01-13T17:00:55+03:00
difficulty: medium
questions: 
notes: 
links: 
---

### Вложенные классы/Nested(static)

Статические вложенные классы (static nested classes) в Java представляют собой внутренние статические классы.

Cтатический вложенный класс ==похож на статический метод==. Слово static перед объявлением класса указывает, что этот ==класс не хранит в себе ссылок на объекты внешнего класса, внутри которого объявлен.==

```java
class Zoo
{
 private static int count = 7;
 private int mouseCount = 1;

 public static int getAnimalCount() //может обращаться к статическим переменным
 {
  return count;
 }

 public int getMouseCount() // может обращаться как к статитеским так  и не статическим
 {
  return mouseCount;
 }

 public static class Mouse // может обращаться только к статическим переменным класса Zoo
 {
  public Mouse()
  {
  }
   public int getTotalCount()
  {
   return **count** + **mouseCount**; //ошибка компиляции.
  }
 }}
```

В отличие от нестатических внутренних классов ==можно создать вложенный класс, даже когда не создан ни один экземпляр внешнего класса.==


Статический внутренний класс в Java **не имеет прямой связи с экземпляром внешнего класса**, поэтому он **не может обращаться к нестатическим (экземплярным) членам внешнего класса напрямую**. Однако он может:

1. **Обращаться к статическим членам внешнего класса** (так как статический класс является частью статического контекста внешнего класса).
2. **Получить доступ к нестатическим членам внешнего класса через экземпляр этого внешнего класса**.


Пример 1: Обращение к статическим членам внешнего класса
```java
class OuterClass {
    private static String staticMember = "I am static";

    static class StaticInnerClass {
        public void printStaticMember() {
            // Доступ к статическим членам внешнего класса
            System.out.println(staticMember);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        OuterClass.StaticInnerClass inner = new OuterClass.StaticInnerClass();
        inner.printStaticMember(); // Вывод: I am static
    }
}

```

#### Пример 2: Доступ к нестатическим членам через экземпляр внешнего класса

```java
class OuterClass {
    private String instanceMember = "I am instance";

    static class StaticInnerClass {
        public void printInstanceMember(OuterClass outer) {
            // Доступ к нестатическим членам через объект внешнего класса
            System.out.println(outer.instanceMember);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.StaticInnerClass inner = new OuterClass.StaticInnerClass();
        inner.printInstanceMember(outer); // Вывод: I am instance
    }
}

```

### Почему так устроено?

- **Статический внутренний класс** создаётся без необходимости создания экземпляра внешнего класса. Это делает его независимым от конкретного экземпляра внешнего класса.
- Нестатические члены внешнего класса привязаны к экземпляру, и статический класс не может к ним обратиться, так как он не знает, с каким экземпляром это связано.

---

### Если нужен доступ ко всем членам внешнего класса

Если статическому внутреннему классу нужно обращаться ко всем членам внешнего класса (включая нестатические), лучше использовать **нестатический внутренний класс**, который автоматически связан с экземпляром внешнего класса:


### К каким методам внешнего класса может обращаться статический внутренний класс?

Статический внутренний класс имеет доступ только к **статическим методам внешнего класса**. Он **не может напрямую вызывать нестатические методы**, так как для вызова нестатических методов требуется конкретный экземпляр внешнего класса.

---

#### 1. **Доступ к статическим методам**

Статический внутренний класс может обращаться к статическим методам внешнего класса напрямую, так как они принадлежат самому классу, а не его экземпляру.



#### 2. **Доступ к нестатическим методам через экземпляр внешнего класса**

Статический внутренний класс не имеет привязки к конкретному экземпляру внешнего класса. Однако он может вызывать нестатические методы внешнего класса, если получает ссылку на его экземпляр.

```java
class OuterClass {
    private void instanceMethod() {
        System.out.println("Instance method in OuterClass");
    }

    static class StaticInnerClass {
        public void callInstanceMethod(OuterClass outer) {
            outer.instanceMethod(); // Доступ к нестатическому методу через экземпляр
        }
    }
}

public class Main {
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.StaticInnerClass inner = new OuterClass.StaticInnerClass();
        inner.callInstanceMethod(outer); // Вывод: Instance method in OuterClass
    }
}

```



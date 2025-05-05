---
title: Enum
tags:
  - JavaSE
related_topics: 
created: 2024-08-30 15:10
modified: 2024-08-30T16:44:47+03:00
difficulty: easy
questions: 
notes: 
links: 
---


![[Pasted image 20240830151208.png]]
![[Pasted image 20240830151304.png]]
![[Pasted image 20240830151450.png]]
![[Pasted image 20240830151905.png]] 

![[Pasted image 20240830152344.png]]

![[Pasted image 20240830152412.png]]
![[Pasted image 20240830152510.png]]


Использование функции в Enum

```java
public enum Operation {
    SUBTRACT {
        public double apply(double x, double y) {
            return x - y;
        }
    },
    SUM {
        public double apply(double x, double y) {
            return x + y;
        }
    },
    MULTIPLY {
        public double apply(double x, double y) {
            return x * y;
        }
    },
    DIVIDE {
        public double apply(double x, double y) {
            return x / y;
        }
    };

    public abstract double apply(double x, double y);
}
```

Пример создания перечисления `MvcCommand` в Java, которое содержит команды для паттерна MVC:

```java
public enum MvcCommand {
    ADD("add"),
    DELETE("delete"),
    UPDATE("update"),
    VIEW("view");

    private final String command;

    MvcCommand(String command) {
        this.command = command;
    }

    public String getCommand() {
        return command;
    }
}
```

Просто Enum перечисление

```java
public enum Season {
    WINTER,
    SPRING,
    SUMMER,
    AUTUMN
}
```
---
title: Switch конструкции
tags:
  - JavaSE
related_topics: 
created: 2024-08-30 15:34
modified: 2024-08-30T16:45:01+03:00
difficulty: 
questions: 
notes: 
links: 
---
# Switch, if else, тернарный оператор

- Switch expression

```java
int dayOfWeek = 3;
switch (dayOfWeek) {
    case 1:
        System.out.println("Понедельник");
        break;
    case 2:
        System.out.println("Вторник");
        break;
    case 3:
        System.out.println("Среда");
        break;
    case 4:
        System.out.println("Четверг");
        break;
    case 5:
        System.out.println("Пятница");
        break;
    default:
        System.out.println("Выходной");
        break;
}
```

- switch с использованием лямдбы

```java
int number = 5;
String result = switch(number) {
    case 1 -> "Один";
    case 2 -> "Два";
    case 3, 4 -> "Три или Четыре";
    case 5 -> {
        String message = "Пять";
        yield message;
    }
    default -> "Другое число";
};
System.out.println(result); // Пять
```

- switch case с использованием Enum

```java
enum DayOfWeek {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

public class Main {
    public static void main(String[] args) {
        DayOfWeek day = DayOfWeek.MONDAY;

        switch (day) {
            case MONDAY:
                System.out.println("It's Monday!");
                break;
            case TUESDAY:
                System.out.println("It's Tuesday!");
                break;
            case WEDNESDAY:
                System.out.println("It's Wednesday!");
                break;
            case THURSDAY:
                System.out.println("It's Thursday!");
                break;
            case FRIDAY:
                System.out.println("It's Friday!");
                break;
            case SATURDAY:
                System.out.println("It's Saturday!");
                break;
            case SUNDAY:
                System.out.println("It's Sunday!");
                break;
            default:
                System.out.println("Invalid day!");
                break;
        }
    }
}
```

- If else

```java
if(true} doSomething() else doAnotherCode();

int a = 1;

if(a == 0){
a++;
}
else if(a == 1){
a--;
}
else{
doSomething();
}
```

- Тернарный оператор

```java
int x = 10>15 ? 100 : 200;
```
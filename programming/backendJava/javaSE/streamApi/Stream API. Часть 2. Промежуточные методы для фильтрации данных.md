---
title: Stream API. Часть 2. Промежуточные методы для фильтрации данных
tags:
  - StreamAPI
related_topics: 
created: 2024-11-14 12:34
modified: 2024-11-14T15:07:14+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20241114123509.png]] 
![[Pasted image 20241114145645.png]]
![[Pasted image 20241114145702.png]]

![[Pasted image 20241114150229.png]]
![[Pasted image 20241114150302.png]]
![[Pasted image 20241114150326.png]]

```java
import java.util.Arrays;
import java.util.stream.Collectors;

public class Task1 {
    public static void main(String[] args) {
        String text = "example of a text with words longer than five letters";
        String result = Arrays.stream(text.split(" "))
                .filter(word -> word.length() <= 5)
                .collect(Collectors.joining(" "));
        
        System.out.println(result);
    }
}

```

```java
public class Task2 {
    public static void main(String[] args) {
        String text = "Th!s t3xt has s0me $pecial @characters!";
        String result = text.chars()
                .filter(Character::isLetter)
                .mapToObj(c -> String.valueOf((char) c))
                .collect(Collectors.joining());
        
        System.out.println(result);
    }
}

```

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

class Cat {
    String name;
    double weight;

    public Cat(String name, double weight) {
        this.name = name;
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "Cat{name='" + name + "', weight=" + weight + "}";
    }
}

public class Task3 {
    public static void main(String[] args) {
        List<Cat> cats = new ArrayList<>();
        cats.add(new Cat("Tom", 2.5));
        cats.add(new Cat("Mittens", 3.2));
        cats.add(new Cat("Whiskers", 4.1));
        cats.add(new Cat("Leo", 2.9));

        List<Cat> result = cats.stream()
                .filter(cat -> cat.weight >= 3)
                .sorted((cat1, cat2) -> cat1.name.compareTo(cat2.name))
                .collect(Collectors.toList());

        result.forEach(System.out::println);
    }
}

```

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Task4 {
    public static void main(String[] args) {
        String[] xmlLines = {
            "<dependency><groupId>junit</groupId><artifactId>junit</artifactId><version>4.4</version><scope>test</scope></dependency>",
            "<dependency><groupId>org.powermock</groupId><artifactId>powermock-reflect</artifactId><version>3.2</version></dependency>"
        };

        List<String> groupIds = Arrays.stream(xmlLines)
                .map(line -> line.replaceAll(".*<groupId>(.+?)</groupId>.*", "$1"))
                .collect(Collectors.toList());

        groupIds.forEach(System.out::println);
    }
}

```

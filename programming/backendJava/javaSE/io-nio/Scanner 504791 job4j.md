---
title: Scanner 504791 job4j
tags:
  - IO-NIO
  - Job4j
related_topics: 
created: 2025-01-20 16:30
modified: 2025-03-25T15:36:29+03:00
questions: 
notes: 
links: 
---

Предназначение

Исходя из названия класса «Scanner», можно догадаться, что этот класс имеет смысл использовать для «сканирования» источника данных. П<mark class="hltr-yellow">од сканированием подразумевается нахождение последовательности символов среди данных источника. </mark>Формально говоря, <mark class="hltr-red">последовательность символов называется токеном или лексемой, а процесс сканирования</mark> [лексическим анализом](https://ru.wikipedia.org/wiki/%D0%9B%D0%B5%D0%BA%D1%81%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9_%D0%B0%D0%BD%D0%B0%D0%BB%D0%B8%D0%B7).

<mark class="hltr-purple">Токенами могут быть примитивы, строки (в значении с англ. line), символьные выражения, соответствующие регулярному выражению и т.п.. В общем все, что может быть представлено в виде последовательности символов.</mark>

Для выделения последовательности символов необходимо знать шаблон, по которому их нужно выделять. В общем случае, это [регулярное выражение](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B3%D1%83%D0%BB%D1%8F%D1%80%D0%BD%D1%8B%D0%B5_%D0%B2%D1%8B%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F). Однако, регулярные выражения трудно читаемы, если у Вас мало навыков работы с ними. В этом случае нам <mark class="hltr-yellow">на помощь приходит Scanner, который поддерживает большинство шаблонов, например, по поиску примитивов. Тем не менее, шаблон есть всегда. Если мы его не задаем, он как-то задается внутри Scanner.</mark>

<mark class="hltr-red">Важно! В Scanner в качестве шаблона задается разделитель между токенами, а не сам шаблон токенов, как при работе с обычными регулярными выражениями.</mark>

Описание

Сам класс работает как Iterator, т.к. поддерживает данный интерфейс.

![[Pasted image 20250120163124.png]]

Причем большинство методов можно разделить на<mark class="hltr-green2"></mark> hasNextTYPE и nextTYPE, где TYPE - э<mark class="hltr-yellow">то тип по шаблону которого будет происходить отделение токенов друг от друга</mark>. Например, hasNextInt(), nextInt().

<mark class="hltr-green2">В качестве источника данных Scanner принимает любой вид данных, включая</mark> Reader, InputStream, File для java.io и Readable, Path для java.util.nio. Также можно задать источник в виде строки String.

Рассмотрим примеры.

Пример #1

Пусть надо из потока данных вычленить только числа. Для этого можно воспользоваться Scanner следующим образом:
```java
package ru.job4j.io.scanner;

import java.io.CharArrayReader;
import java.util.Scanner;

public class ScannerExample1 {
    public static void main(String[] args) {
        var ls = System.lineSeparator();
        var data = String.join(ls,
                "1 A 2",
                "3 4 B",
                "C 5 6"
        );
        var scanner = new Scanner(new CharArrayReader(data.toCharArray()));
        while (scanner.hasNext()) {
            if (scanner.hasNextInt()) {
                System.out.print(scanner.nextInt());
                System.out.print(" ");
            } else {
                scanner.next();
            }
        }
    }
}
```

![[Pasted image 20250120163152.png]]
Обратите внимание, что здесь<mark class="hltr-yellow"> в качестве источника мы указали одну из реализаций Reader - CharArrayReader</mark>. Данная реализация позволяет читать данные из массива символов, т.е. из временной памяти. 

<mark class="hltr-red">Важно! В примере выше шаблон разделителя не указан, поэтому используется тот, что по умолчанию, а именно символы перехода на новую строку и пробел.</mark>

Пример #2

Пусть надо из потока данных вычленить адреса почты, которые разделены между собой “, ”. Можно поступить так:
```java
package ru.job4j.io.scanner;

import java.io.ByteArrayInputStream;
import java.util.Scanner;

public class ScannerExample2 {
    public static void main(String[] args) {
        var data = "empl1@mail.ru, empl2@mail.ru, empl3@mail.ru";
        var scanner = new Scanner(new ByteArrayInputStream(data.getBytes()))
                .useDelimiter(", ");
        while (scanner.hasNext()) {
            System.out.println(scanner.next());
        }
    }
}
```

![[Pasted image 20250120163214.png]]

Обратите внимание, что здесь в качестве источника мы указали одну из реализаций InputStream – ByteArrayInputStream. <mark class="hltr-orange">В качестве разделителя с помощью метода useDelimiter() мы указали нужный разделитель.
</mark>
В данном случае мы могли воспользоваться методом String.split(), но<mark class="hltr-green2"> когда дело доходит до чтения файлов, то Scanner позволяет использовать преимущества буферизированных потоков и возможности по разбиению на токены.</mark>

Пример #3

<mark class="hltr-red">Еще одной интересной возможностью Scanner является возможность задать систему счисления при чтении чисел. Например, можно прочитать числа в шестнадцатеричном виде и вывести в десятичном таким образом</mark>:
```java
package ru.job4j.io.scanner;

import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.util.Scanner;

public class ScannerExample3 {
    public static void main(String[] args) throws Exception {
        var data = "A 1B FF 110";
        var file = File.createTempFile("data", null);
        try (BufferedOutputStream output = new BufferedOutputStream(new FileOutputStream(file))) {
            output.write(data.getBytes());
        }
        try (var scanner = new Scanner(file).useRadix(16)) {
            while (scanner.hasNextInt()) {
                System.out.print(scanner.nextInt());
                System.out.print(" ");
            }
        }
    }
}
```

![[Pasted image 20250120163238.png]]

<mark class="hltr-red">Обратите внимание, что теперь мы указываем в качестве источника данных временный файл, который создаем и в который записываем предварительно. Метод useRadix() указывает в какой системе счисления находятся входные числа.</mark>

<mark class="hltr-red">Важно! Если Scanner работает с внешними источниками его нужно использовать с try-with-resources.</mark>

Заключение

Класс java.util.<mark class="hltr-green2">Scanner может быть полезен, когда нужно вычленить из данных только те, что Вам нужны. Для этого нужно назначить разделитель, например, пробел, запятая или регулярное выражение. Также Scanner имеет полезную особенность для чтения чисел различных систем счисления. </mark>

Полезные ссылки

1. [https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/Scanner.html](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/Scanner.html) 

2. [https://java-master.com/%D0%BA%D0%BB%D0%B0%D1%81%D1%81-scanner-%D0%B2-java/](https://java-master.com/%D0%BA%D0%BB%D0%B0%D1%81%D1%81-scanner-%D0%B2-java/) 
 
3. [https://www.baeldung.com/java-scanner](https://www.baeldung.com/java-scanner)

![[Pasted image 20250120165409.png]]

![[Pasted image 20250120165420.png]]


------

![[Pasted image 20250120163304.png]]
![[Pasted image 20250120163317.png]]

```java
package ru.job4j.io;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.PrintStream;
import java.util.ArrayList;
import java.util.Scanner;
import java.util.StringJoiner;

public class CSVReader {
    public static void handle(ArgsName argsName) throws Exception {
    }
    
    public static void main(String[] args) {
        /* здесь добавьте валидацию принятых параметров*/
        ArgsName argsName = ArgsName.of(args);
        handle(argsName);
    }
}
```

```java
package ru.job4j.io;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.io.TempDir;
import static org.assertj.core.api.AssertionsForClassTypes.assertThat;
import java.io.File;
import java.nio.file.Path;
import java.nio.file.Files;

class CSVReaderTest {

    @Test
    void whenFilterTwoColumns(@TempDir Path folder) throws Exception {
        String data = String.join(
                System.lineSeparator(),
                "name;age;last_name;education",
                "Tom;20;Smith;Bachelor",
                "Jack;25;Johnson;Undergraduate",
                "William;30;Brown;Secondary special"
        );
        File file = folder.resolve("source.csv").toFile();
        File target = folder.resolve("target.csv").toFile();
        ArgsName argsName = ArgsName.of(new String[]{
                "-path=" + file.getAbsolutePath(), "-delimiter=;",
                "-out=" + target.getAbsolutePath(), "-filter=name,education"});
        Files.writeString(file.toPath(), data);
        String expected = String.join(
                System.lineSeparator(),
                "name;education",
                "Tom;Bachelor",
                "Jack;Undergraduate",
                "William;Secondary special"
        ).concat(System.lineSeparator());
        CSVReader.handle(argsName);
        assertThat(Files.readString(target.toPath())).isEqualTo(expected);
    }

    @Test
    void whenFilterThreeColumns(@TempDir Path folder) throws Exception {
        String data = String.join(
                System.lineSeparator(),
                "name,age,last_name,education",
                "Tom,20,Smith,Bachelor",
                "Jack,25,Johnson,Undergraduate",
                "William,30,Brown,Secondary special"
        );
        File file = folder.resolve("source.csv").toFile();
        File target = folder.resolve("target.csv").toFile();
        ArgsName argsName = ArgsName.of(new String[]{
                "-path=" + file.getAbsolutePath(), "-delimiter=,",
                "-out=" + target.getAbsolutePath(), "-filter=education,age,last_name"
        });
        Files.writeString(file.toPath(), data);
        String expected = String.join(
                System.lineSeparator(),
                "education,age,last_name",
                "Bachelor,20,Smith",
                "Undergraduate,25,Johnson",
                "Secondary special,30,Brown"
        ).concat(System.lineSeparator());
        CSVReader.handle(argsName);
        assertThat(Files.readString(target.toPath())).isEqualTo(expected);
    }
}
```


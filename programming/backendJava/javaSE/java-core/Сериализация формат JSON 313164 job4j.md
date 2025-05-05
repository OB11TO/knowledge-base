---
title: Сериализация формат JSON 313164 job4j
tags:
  - JavaSE
  - Job4j
related_topics: 
created: 2025-01-23 14:47
modified: 2025-01-23T15:18:18+03:00
questions: 
notes: 
links: 
---

![[Pasted image 20250123144834.png]]

**JSON (JavaScript Object Notation)** – <mark class="hltr-yellow">текстовый формат обмена данными, основан на синтаксисе JavaScript</mark>, удобен для написания и чтения человеком. Несмотря на происхождение от JavaScript формат независим от него и может использоваться практически с любым языком программирования, для многих из которых существуют готовые библиотеки для создания и обработки данных в формате JSON.

Применение:

- наиболее часто<mark class="hltr-red"> применяется для пересылки информации между браузером и сервером </mark>(загрузка контента по технологии Ajax) или между веб-сайтами (различные HTTP-соединения).
- <mark class="hltr-green2">можно использовать везде, где требуется обмен данных между программами;</mark>
- <mark class="hltr-yellow">хранение локальной информации (</mark>например, настроек);
- за счёт лаконичности м<mark class="hltr-green2">ожет быть выбран для сериализации сложных структур вместо XML</mark>.

Примитивные типы данных в JSON:

- число (целое или вещественное)
- литералы true, false и null
- строка —символы юникода, заключённые в двойные кавычки

Ссылочные типы данных:

- Объект - заключается в фигурные скобки ({ и }) и содержит разделенный запятой список пар имя/значение.
- Массив - заключается в квадратные скобки ([ и ]) и содержит разделенный запятой список значений.


-----

В java существует много библиотек для работы с json, одни из<mark class="hltr-orange"> наиболее популярных Jackson и Gson позволяют преобразовывать json-строку сразу в объект и наоборот.</mark> Рассмотрим на предыдущем примере работу с библиотекой Gson:

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.11.0</version>
</dependency>
```

```java
package ru.job4j.serialization.json;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class Main {
    public static void main(String[] args) {
        final Person person = new Person(false, 30, new Contact("11-111"),
                new String[] {"Worker", "Married"});

        /* Преобразуем объект person в json-строку. */
        final Gson gson = new GsonBuilder().create();
        System.out.println(gson.toJson(person));

        /* Создаём новую json-строку с модифицированными данными*/
        final String personJson =
                "{"
                        + "\"sex\":false,"
                        + "\"age\":35,"
                        + "\"contact\":"
                        + "{"
                        + "\"phone\":\"+7(924)111-111-11-11\""
                        + "},"
                        + "\"statuses\":"
                        + "[\"Student\",\"Free\"]"
                        + "}";
        /* Превращаем json-строку обратно в объект */
        final Person personMod = gson.fromJson(personJson, Person.class);
        System.out.println(personMod);
    }
}

```

Gson как и Jackson предоставляют возможности гибкой настройки механизмов преобразования используя аннотации, свои классы-правила сериализации и десериализации и пр.
![[Pasted image 20250123151801.png]]


---


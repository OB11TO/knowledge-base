---
title: Yaml
tags:
  - ФорматДанных
  - JavaEE
related_topics: 
created: 2024-09-11 13:53
modified: 2024-11-07T13:09:27+03:00
questions: 
notes: 
links: 
---

### Yaml

- YAML (Yet Another Markup Language) - это формат сериализации данных, который обычно используется для конфигурационных файлов.
- YAML представляет данные в виде иерархической структуры с помощью отступов:

```YAML
ключ: значение
другой_ключ:
  - элемент1
  - элемент2
```

- Значения могут быть строками, числами, логическими значениями, массивами, объектами или специальными значениями `null`.
- Списки элементов указываются с помощью дефиса и отступов.

### Yaml Parser

Зависимости

```XML
<dependencies>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-yaml -->
        <dependency>
            <groupId>com.fasterxml.jackson.dataformat</groupId>
            <artifactId>jackson-dataformat-yaml</artifactId>
            <version>2.15.2</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.14.2</version>
        </dependency>
    </dependencies>
```

Класс для парсинга

```Java
package ru.konovalov.url;

public class Book {
    private String title;
    private String author;
    private int pages;

    public Book(String title, String author, int pages) {
        this.title = title;
        this.author = author;
        this.pages = pages;
    }

    public Book() {
    }

    @Override
    public String toString() {
        return "Book{" +
               "title='" + title + '\'' +
               ", author='" + author + '\'' +
               ", pages=" + pages +
               '}';
    }
}
```

Парсер

```Java
package  ru.konovalov.url;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.dataformat.yaml.YAMLMapper;

public class YamlParser {
    public static void parseYAML(Book book) throws JsonProcessingException {
        ObjectMapper mapper = new YAMLMapper();
        mapper.enable(SerializationFeature.INDENT_OUTPUT);
        String yamlBook = mapper.writeValueAsString(book);
        System.out.println(yamlBook);
    }
    public static void deparseYAML(String yamlString) throws JsonProcessingException {
        ObjectMapper mapper = new YAMLMapper();
        Book book = mapper.readValue(yamlString, Book.class);
        System.out.println(book);
    }
}
```

Использование парсера

```Java
package ru.konovalov.url;

import com.fasterxml.jackson.core.JsonProcessingException;

public class Runner {
    public static void main(String[] args) {
        String yamlString =  """
           ---
           title: "Обитаемый остров"
           author: "Стругацкий А., Стругацкий Б."
           pages: 413""";

        Book book = new Book("Обитаемый остров", "Стругацкий А., Стругацкий Б.", 413);

        try {

            YamlParser.parseYAML(book);
            YamlParser.deparseYAML(yamlString);

        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }

    }
}
```

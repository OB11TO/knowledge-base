---
title: JSON
tags:
  - ФорматДанных
  - JavaEE
related_topics: 
created: 2024-09-11 13:53
modified: 2024-11-07T13:09:10+03:00
questions: 
notes: 
links: 
---
### JSON

JSON:

- JSON (JavaScript Object Notation) - это формат обмена данными, основанный на синтаксисе JavaScript.
- JSON представляет данные в виде пар "ключ-значение":

```JSON
{
  "ключ": "значение",
  "ключ2": "значение2"
}
```

- Ключи и значения должны быть заключены в двойные кавычки.
- Различные пары "ключ-значение" разделяются запятой.
- Значения могут быть строками, числами, логическими значениями, массивами, объектами или специальными значениями `null`.
- JSON не поддерживает комментарии.

```JSON
{
"id": 25,
"startRow": {
	"method": "GET",
	"url": "www.google.com",
	"version": 1.1
	},
"headers": [
		{
			"name": "accept",
			"value" : "text/html"
		},
		{
			"name": "accept",
			"value" : "text/json"
		}
	],
"body": {
	"type": "text/plain"
	}
}
```

### JSON Parser

Необходимо добавить зависимости:

```XML
<dependencies>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.14.2</version>
        </dependency>
    </dependencies>
```

Класс для парсинга

```Java
import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonInclude;
import com.fasterxml.jackson.annotation.JsonPropertyOrder;
import lombok.Data;
import lombok.Getter;


@Data
@JsonInclude(JsonInclude.Include.NON_DEFAULT)
@JsonPropertyOrder({"author", "title", "pages"})
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

aaa

```Java
import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.DeserializationFeature;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

public class JSONParser {
    public static void parseJson(Book book) throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper()
                .setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY)
                .enable(SerializationFeature.INDENT_OUTPUT);
        String jsonBook =  mapper.writeValueAsString(book);
        System.out.println(jsonBook);
    }

    public static void deparseJson(String jsonString) throws JsonProcessingException {
        Book book1 = new ObjectMapper()
                .setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY)
                .configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false)
                .readValue(jsonString, Book.class);
        System.out.println(book1);
    }
}
```

Реализация парсинга

```Java
package ru.konovalov.url;

import com.fasterxml.jackson.core.JsonProcessingException;

public class Runner {
    public static void main(String[] args) {
        String jsonString = """
                {
                 	"title" : "Обитаемый остров",
                 	"author" : "Стругацкий А., Стругацкий Б.",
                 	"pages" : 413,
                 	"unknown property" : 42
                }""";

        Book book = new Book("Обитаемый остров", "Стругацкий А., Стругацкий Б.", 413);

        try {

						JsonParser.parseJson(book);
            JsonParser.deparseJson(jsonString);

        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }

    }
}
```

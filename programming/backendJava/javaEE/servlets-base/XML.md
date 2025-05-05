---
title: XML
tags:
  - ФорматДанных
  - JavaEE
related_topics: 
created: 2024-09-11 13:51
modified: 2025-01-23T15:46:40+03:00
questions: 
notes: 
links: 
---

### XML

XML:

- XML (Расширяемый язык разметки) - это язык разметки, который используется для хранения и передачи структурированных данных.
- XML-документ должен начинаться с объявления версии и кодировки, которые указываются в заголовке:

```XML
<?xml version="1.0" encoding="UTF-8"?>
```

- Данные в XML представляются в виде элементов, заключенных в открывающие и закрывающие теги:

```XML
<элемент>данные</элемент>
```

- Элементы могут содержать атрибуты, которые указываются внутри открывающего тега:

```XML
<элемент атрибут="значение">данные</элемент>
```

- Вложенные элементы должны быть правильно вложены друг в друга:

```XML
<родительский>
  <дочерний>данные</дочерний>
</родительский>
```

- Специальные символы, такие как `<`, `>`, `&`, должны быть заменены соответствующими символами-сущностями:

```XML
<элемент>значение &gt; 5</элемент>
```

![[Untitled 39.png]]

Тег может либо содержать текстовый узел, либо другие теги, либо быть пустым в примере - <body/>

```XML
<?xml version="1.0" encoding="UTF-8" ?>
<request id ="25">
    <start-row>
        <url>www.google.com</url>
    </start-row>
    <headers>
        <header>
            <name>content-type</name>
            <value>text/html</value>
        </header>
        <header>
            <name>accept</name>
            <value>application/json</value>
        </header>
    </headers>
    <body type = "text.plain"/> --пустое поле
</request>
```

### XML Parser

Необходимо добавить зависимости:

```XML
<dependencies>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
        <dependency>
            <groupId>com.fasterxml.jackson.dataformat</groupId>
            <artifactId>jackson-dataformat-xml</artifactId>
            <version>2.15.2</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.14.2</version>
        </dependency>
    </dependencies>
```

Класс для парсинга:

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

Парсер:

```Java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.dataformat.xml.XmlMapper;

public class XMLParser {
    public static void parseXML(Book book) throws JsonProcessingException {
        ObjectMapper mapper = new XmlMapper();
        mapper.enable(SerializationFeature.INDENT_OUTPUT);
        String xmlBook = mapper.writeValueAsString(book);
        System.out.println(xmlBook);
    }

    public static void deparseXML(String xmlString) throws JsonProcessingException {
        ObjectMapper mapper = new XmlMapper();
        Book book = mapper.readValue(xmlString, Book.class);
        System.out.println(book);
    }
}
```

Реализация парсинга:

```Java
import com.fasterxml.jackson.core.JsonProcessingException;


public class Runner {

    public static void main(String[] args) throws JsonProcessingException {

        String xmlString = """
                 <Book>
                  <title>Обитаемый остров</title>
                  <author>Стругацкий А., Стругацкий Б.</author>
                  <pages>413</pages>
                </Book>""";

        Book book = new Book("Обитаемый остров", "Стругацкий А., Стругацкий Б.", 413);

        try {
            XMLParser.parseXML(book);
            XMLParser.deparseXML(xmlString);
        }
        catch (JsonProcessingException e){
            System.out.println("Ошибка в методах обработки данных");
        }
    }
}
```

  
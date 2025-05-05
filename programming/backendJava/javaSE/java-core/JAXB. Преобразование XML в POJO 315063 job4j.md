---
title: JAXB. Преобразование XML в POJO 315063 job4j
tags:
  - JavaSE
  - Job4j
related_topics: 
created: 2025-01-23 16:06
modified: 2025-01-23T16:14:17+03:00
questions: 
notes: 
links: 
---


![[Pasted image 20250123160641.png]]

**POJO (Plain Old Java Object)** — «старый добрый Java-объект», простой Java-объект. Термин впервые начал употребляться Мартином Фаулером и его коллегами как результат поиска способов упрощения разработки. Нет формального определения, какие объекты являются POJO, обычно руководствуются следующими соглашениями для класса:

- <mark class="hltr-green2">не наследуется от других классов</mark> (возможно, кроме POJO-классов того же пакета)
- <mark class="hltr-yellow">не реализует интерфейсов</mark> (иногда делается исключение для маркерных интерфейсов из стандартной библиотеки, или тех, которые нужны для бизнес-модели),
- <mark class="hltr-purple">не использует аннотаций в определениях</mark>
- не зависит от сторонних библиотек.

Иногда под POJO подразумевают JavaBean, но JavaBeans имеют более строгие ограничения.
1. <mark class="hltr-red">Для начала нам нужно добавить зависимости на библиотеку JAXB с помощью которой мы будет делать преобразования</mark>
```java
    <dependency>
      <groupId>jakarta.xml.bind</groupId>
      <artifactId>jakarta.xml.bind-api</artifactId>
      <version>4.0.2</version>
    </dependency>
     <dependency>
      <groupId>jakarta.activation</groupId>
      <artifactId>jakarta.activation-api</artifactId>
      <version>2.1.3</version>
    </dependency>
    <dependency>
      <groupId>org.glassfish.jaxb</groupId>
      <artifactId>jaxb-runtime</artifactId>
      <version>4.0.5</version>
    </dependency>
```

2. Для использования JAXB нам нужно в модели добавить дефолтные конструкторы. С полей для простоты также уберем final. Получим такие модели:
```java
package ru.job4j.io.serialization.xml;


public class Person {

    private boolean sex;

    private int age;

    private Contact contact;

    private String[] statuses;

    public Person() { }

    public Person(boolean sex, int age, Contact contact, String... statuses) {
        this.sex = sex;
        this.age = age;
        this.contact = contact;
        this.statuses = statuses;
    }

    @Override
    public String toString() {
        return "Person{"
                + "sex=" + sex
                + ", age=" + age
                + ", contact=" + contact
                + ", statuses=" + Arrays.toString(statuses)
                + '}';
    }

}



```

```java
package ru.job4j.io.serialization.xml;

public class Contact {

    private String phone;

    public Contact() {

    }

    public Contact(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "Contact{"
                + "phone='" + phone + '\''
                + '}';
    }
}
```

3. <mark class="hltr-orange">Для того чтобы сериализовать и десериализовать нам нужно добавить аннотации JAXB, которая даст библиотеке информацию о том как парсить объект.</mark>

1) Как вы уже знаете <mark class="hltr-yellow">xml обязательно должен иметь корневой тег, </mark>в котором все и будет располагаться. <mark class="hltr-blue">Для его обозначения служит@XmlRootElemen</mark>t. Эту аннотацию нужно ставить над сущностью, которая будет корневой в нашем случае это Person.

2) <mark class="hltr-green2">Над вложенными сущностями</mark> нам нужно поставить просто <mark class="hltr-red">@XmlElement</mark>

3) Для того чтобы<mark class="hltr-yellow"> поле считалось атрибутом нужно поставить </mark> <mark class="hltr-orange">@XmlAttribute</mark>, по умолчанию поле парсится как тег

4) Мы можем <mark class="hltr-yellow">указать также как мы хотим читать/писать объект. По геттерам/сеттерам или напрямую по полям (</mark>используется рефлексия). Мы будем использовать доступ по полям. Для этих целей служит аннотация <mark class="hltr-red">@XmlAccessorType</mark>

В соответствии с этим добавим аннотации
```java
package ru.job4j.io.serialization.xml;

import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.JAXBException;
import jakarta.xml.bind.Marshaller;
import jakarta.xml.bind.annotation.*;
import java.io.StringWriter;
import java.util.Arrays;

@XmlRootElement(name = "person")
@XmlAccessorType(XmlAccessType.FIELD)
public class Person {

    @XmlAttribute
    private boolean sex;

    @XmlAttribute
    private int age;
    private Contact contact;

    private String[] statuses;

    public Person() { }

    public Person(boolean sex, int age, Contact contact, String... statuses) {
        this.sex = sex;
        this.age = age;
        this.contact = contact;
        this.statuses = statuses;
    }

    @Override
    public String toString() {
        return "Person{"
                + "sex=" + sex
                + ", age=" + age
                + ", contact=" + contact
                + ", statuses=" + Arrays.toString(statuses)
                + '}';
    }



    public static void main(String[] args) throws JAXBException {

        final Person person = new Person(false, 30, new Contact("11-111"), "Worker", "Married");

        JAXBContext context = JAXBContext.newInstance(Person.class);
        Marshaller marshaller = context.createMarshaller();
        marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.TRUE);
        
        try (StringWriter writer = new StringWriter()) {
            marshaller.marshal(person, writer);
            String result = writer.getBuffer().toString();
            System.out.println(result);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}
```

```java
package ru.job4j.io.serialization.xml;

import jakarta.xml.bind.annotation.XmlAttribute;

@XmlRootElement(name = "contact")
public class Contact {

    @XmlAttribute
    private String phone;

    public Contact() {

    }

    public Contact(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "Contact{"
                + "phone='" + phone + '\''
                + '}';
    }
}
```

```java
package ru.job4j.io.serialization.xml;

import jakarta.xml.bind.JAXBContext;
import jakarta.xml.bind.Marshaller;
import jakarta.xml.bind.Unmarshaller;
import java.io.StringReader;
import java.io.StringWriter;

public class Main {
    public static void main(String[] args) throws Exception {
        Person person = new Person(false, 30, new Contact("11-111"), "Worker", "Married");
        /* Получаем контекст для доступа к АПИ */
        JAXBContext context = JAXBContext.newInstance(Person.class);
        /* Создаем сериализатор */
        Marshaller marshaller = context.createMarshaller();
        /* Указываем, что нам нужно форматирование */
        marshaller.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.TRUE);
        String xml = "";
        try (StringWriter writer = new StringWriter()) {
            /* Сериализуем */
            marshaller.marshal(person, writer);
            xml = writer.getBuffer().toString();
            System.out.println(xml);
        }
        /* Для десериализации нам нужно создать десериализатор */
        Unmarshaller unmarshaller = context.createUnmarshaller();
        try (StringReader reader = new StringReader(xml)) {
            /* десериализуем */
            Person result = (Person) unmarshaller.unmarshal(reader);
            System.out.println(result);
        }

    }
}
```

![[Pasted image 20250123161400.png]]

```java
@XmlElementWrapper(name = "statuses")
@XmlElement(name = "status")
private String[] statuses;
```


-----


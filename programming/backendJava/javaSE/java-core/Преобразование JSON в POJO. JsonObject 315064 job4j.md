---
title: Преобразование JSON в POJO. JsonObject 315064 job4j
tags:
  - JavaSE
  - Job4j
related_topics: 
created: 2025-01-23 16:35
modified: 2025-01-23T16:39:06+03:00
questions: 
notes: 
links: 
---

![[Pasted image 20250123163550.png]]


**POJO (Plain Old Java Object)** — «старый добрый Java-объект», простой Java-объект. Термин впервые начал употребляться Мартином Фаулером и его коллегами как результат поиска способов упрощения разработки. Нет формального определения, какие объекты являются POJO, обычно руководствуются следующими соглашениями для класса:

- не наследуется от других классов (возможно, кроме POJO-классов того же пакета)
- не реализует интерфейсов (иногда делается исключение для маркерных интерфейсов из стандартной библиотеки, или тех, которые нужны для бизнес-модели),
- не использует аннотаций в определениях
- не зависит от сторонних библиотек.

Иногда под POJO подразумевают JavaBean, но JavaBeans имеют более строгие ограничения.

**JSON-Java (org.json)** легковесная функциональная библиотека для работы с JSON, которая дополнительно умеет преобразовывать JSON в XML, HTTP header, Cookies и др. В отличие от Jackson или Gson, JSON-Java преобразует json-строку не в объект пользовательского класса (способ Data bind), а в объекты своей библиотеки JSONObject, JSONArray (способ Tree Model).

```xml
<dependency>
    <groupId>org.json</groupId>
    <artifactId>json</artifactId>
    <version>20240303</version>
</dependency>
```

Объекты классов Person, Contact из урока «Формат JSON» являются POJO, но для корректного преобразования в строку с помощью org.json к ним ещё _**необходимо добавить геттеры**_. <mark class="hltr-red">Повторим, потому что это важно: каждый объект, который планируется преобразовывать в строку с помощью org.json, а также объекты, которые являются составными частями этого объекта, должны иметь геттеры на все свои поля.</mark>

```java
public static void main(String[] args) {

    /* JSONObject из json-строки строки */
    JSONObject jsonContact = new JSONObject("{\"phone\":\"+7(924)111-111-11-11\"}");

    /* JSONArray из ArrayList */
    List<String> list = new ArrayList<>();
    list.add("Student");
    list.add("Free");
    JSONArray jsonStatuses = new JSONArray(list);

    /* JSONObject напрямую методом put */
    final Person person = new Person(false, 30, new Contact("11-111"), "Worker", "Married");
    JSONObject jsonObject = new JSONObject();
    jsonObject.put("sex", person.getSex());
    jsonObject.put("age", person.getAge());
    jsonObject.put("contact", jsonContact);
    jsonObject.put("statuses", jsonStatuses);

    /* Выведем результат в консоль */
    System.out.println(jsonObject.toString());

    /* Преобразуем объект person в json-строку */
    System.out.println(new JSONObject(person).toString());
}

```


-----

**Пример рекурсивного зацикливания.**

При преобразовании объектов в json-строку возможно рекурсивное зацикливание, простейший пример, когда объект A содержит ссылку на объект B, а он в свою очередь ссылается на первоначальный объект A. При выполнении будут осуществляться переходы по ссылке и сериализация до возникновения исключения _StackOverflowError_. Чтобы избежать исключения необходимо разорвать цепочку, как правило это делается в момент перехода по ссылке на объект, который уже сериализован. В библиотеке JSON-Java (org.json) для этого существует аннотация _@<mark class="hltr-red">JSONPropertyIgnore</mark>:_

```java
package ru.job4j.serialization.json;

import org.json.JSONPropertyIgnore;

public class A {
    private B b;

    @JSONPropertyIgnore
    public B getB() {
        return b;
    }

    public void setB(B b) {
        this.b = b;
    }

    public String getHello() {
        return "Hello";
    }

}
```

```java
package ru.job4j.serialization.json;

import org.json.JSONObject;

public class B {
    private A a;

    public A getA() {
        return a;
    }

    public void setA(A a) {
        this.a = a;
    }

    public static void main(String[] args) {
        A a = new A();
        B b = new B();
        a.setB(b);
        b.setA(a);

        System.out.println(new JSONObject(b));
    }
}

```


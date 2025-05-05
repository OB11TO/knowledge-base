---
title: Сериализация job4j
tags:
  - JavaSE
  - Job4j
related_topics: 
created: 2025-01-23 14:34
modified: 2025-01-23T16:35:39+03:00
questions: 
notes: 
links: 
---



-----
[[Сериализация формат JSON 313164 job4j]]
[[Сериализация формат XML 313165 job4j]]
[[JAXB. Преобразование XML в POJO 315063 job4j]]
[[Преобразование JSON в POJO. JsonObject 315064 job4j]]


----

![[Pasted image 20250123143423.png]]

```java
package ru.job4j.serialization.java;

import java.io.*;
import java.nio.file.Files;

public class Contact implements Serializable {
    private static final long serialVersionUID = 1L;
    private final int zipCode;
    private final String phone;

    public Contact(int zipCode, String phone) {
        this.zipCode = zipCode;
        this.phone = phone;
    }

    public int getZipCode() {
        return zipCode;
    }

    public String getPhone() {
        return phone;
    }

    @Override
    public String toString() {
        return "Contact{" +
                "zipCode=" + zipCode +
                ", phone='" + phone + '\'' +
                '}';
    }

    public static void main(String[] args) throws IOException, ClassNotFoundException {
        final Contact contact = new Contact(123456, "+7 (111) 111-11-11");

        /* Запись объекта во временный файл, который удалится системой */
        File tempFile = Files.createTempFile(null, null).toFile();
        try (FileOutputStream fos = new FileOutputStream(tempFile);
             ObjectOutputStream oos =
                     new ObjectOutputStream(fos)) {
            oos.writeObject(contact);
        }

        /* Чтение объекта из файла */
        try (FileInputStream fis = new FileInputStream(tempFile);
             ObjectInputStream ois =
                     new ObjectInputStream(fis)) {
            final Contact contactFromFile = (Contact) ois.readObject();
            System.out.println(contactFromFile);
        }
    }
}


```


![[Pasted image 20250123144002.png]]

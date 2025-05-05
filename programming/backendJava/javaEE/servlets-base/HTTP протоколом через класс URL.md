---
title: HTTP протоколом через класс URL
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:56
modified: 2024-09-13T18:15:38+03:00
questions: 
notes: 
links: 
---

### Пример работы с HTTP протоколом через класс URL

Удобно работать только с GET запросами. C POST запросами во первых приходится разрешить URLconnection.setDoOutput(true), так как изначально он выставлен в false. Потом переводить данные в байты и работать через OutPutStream

```Java
package ru.konovalov.url;

import java.io.IOException;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;

public class UrlExample {
    public static void main(String[] args) throws IOException {
        URL url = new URL("file:/home/vadim/IdeaProjects/http-servlets-starter/src/main/java/ru/konovalov/DatagramRunner.java");
        URLConnection urlConnection = url.openConnection();
        System.out.println(new String(urlConnection.getInputStream().readAllBytes())); // вывод полной информации о нашем файле


         */
    }

    private static void checkGoogle() throws IOException {
        URL url = new URL("https://www.google.com");
        URLConnection connection = url.openConnection();
//ниже выведем html страницу по адресу  "https://www.google.com"( get запрос) 
				System.out.println(connection.getInputStream.readAllBytes());
//ниже представлен постзапрос
        connection.setDoOutput(true);
        try (OutputStream outputStream = connection.getOutputStream()) {
            outputStream.write(new byte[512]);
        }
    }
}
```

---
title: GET и POST запроса с помощью HTTPClient
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:57
modified: 2024-09-13T18:16:23+03:00
questions: 
notes: 
links: 
---

### Пример GET и POST запроса с помощью HTTPClient

- Помимо метода send() в данном классе имеется sendAsync() возвращающий CompletableFuture<HttpResponse\<T>> позволяющий не останавливать выполнение потока из-за ожидания response.

```Java
package ru.konovalov.url;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.file.Path;

public class HTTPClientExample {
    public static void main(String[] args) throws IOException, InterruptedException {
        HttpClient client = HttpClient.newBuilder()
                .version(HttpClient.Version.HTTP_1_1)  // по умолчанию версия 2
                .build();
        //GET запрос
        HttpRequest request = HttpRequest.newBuilder(URI.create("https://www.google.com"))
                .GET()
                .build();
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
        System.out.println(response.headers()); // вернет html страницу

				//POST запрос
        HttpRequest request2 = HttpRequest.newBuilder(URI.create("https://www.google.com"))
                .POST(HttpRequest.BodyPublishers.ofFile(Path.of("path", "to", "file")))
                .build();
    }
}
```

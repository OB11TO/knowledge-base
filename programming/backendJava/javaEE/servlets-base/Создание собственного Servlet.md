---
title: Создание собственного Servlet
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:57
modified: 2024-09-11T13:57:43+03:00
questions: 
notes: 
links: 
---
### Создание собственного Servlet


- Создание папки lib в корне проекта → Add as Library и перенести в эту директорую jar файл (либо зависимость в pom jakarta.servlet-api)

```Bash
cp servlet-api.jar /home/vadim/IdeaProjects/http-servlets-starter/lib
```

- Создать свой класс extends HtttpServlet из servlet-api.jar

```Java
package servlet;

import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet("/first")
public class FirstServlet extends HttpServlet {

    @Override
    public void init(ServletConfig config) throws ServletException {

        super.init(config);
    }

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.service(req, resp);
    }

    @Override
    public void destroy() {
        super.destroy();
    }
}
```

Хорошим тоном является переопределение не метода service, а конкретных методов:

![[Untitled 45.png]]

```Java
package servlet;

import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/first")
public class FirstServlet extends HttpServlet {

    @Override
    public void init(ServletConfig config) throws ServletException {

        super.init(config);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      // позволяет вывести в консоль с какого устройства был выполнен запрос
			System.out.println(req.getHeader("user-agent"));
			
			resp.setContentType("text/html");

//инициализирует нужную кодировку
			resp.setCharacterEncoding(StandardCharsets.UTF_8.name());

//либо так resp.setContentType("text/html" ; "charset-UTF-8");

//добавляет кастомный header
		resp.setHeader("token", "12345");


        try (PrintWriter writer = resp.getWriter()) {
            writer.write("<h1>Hello from first Servlet</h1>");
        }
        super.doGet(req, resp);
    }

    @Override
    public void destroy() {
        super.destroy();
    }
}
```

После запуска томкэта на порту 8081 http://localhost:8081/first

![[Untitled 46.png]]

![[Untitled 47.png]]

![[Untitled 48.png]]

  

Еще один пример. Реализация get метода со скачиванием текстового файла, который содержит информацию о переданном json файле.

```Java
package servlet;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

@WebServlet("/download")
public class DownloadServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setHeader("Content-Disposition", "attachment; filename=\"filename.txt\"");
        resp.setContentType("application/json");
        resp.setCharacterEncoding(StandardCharsets.UTF_8.name());
        try(var printWriter = resp.getOutputStream();
        var stream = DownloadServlet.class.getClassLoader().getResourceAsStream("first.json"))
        {
            printWriter.write(stream.readAllBytes());
        }
    }


}
```

localhost:8081/download

![[Untitled 49.png]]

  

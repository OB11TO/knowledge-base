---
title: Отображение ошибки в виде jsp страницы
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 14:05
modified: 2024-09-11T14:05:47+03:00
questions: 
notes: 
links: 
---
### Отображение ошибки в виде jsp страницы

error,jsp:

```Java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    Error! Ooops
</body>
</html>
```

web.xml:

```Java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <error-page>
        <exception-type>java.lang.Throwable</exception-type>
        <location>/WEB-INF/jsp/error.jsp</location>
    </error-page>
</web-app>
```

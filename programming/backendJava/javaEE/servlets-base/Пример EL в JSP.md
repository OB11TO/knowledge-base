---
title: Пример EL в JSP
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 14:02
modified: 2024-09-11T14:02:48+03:00
questions: 
notes: 
links: 
---
### Пример EL в JSP

```Java

@WebServlet("/content")
public class ContentServlet extends HttpServlet {
    private static final ArrayList<Object> flights = new ArrayList<>(10){{
        add(new Object());
        add(new Object());
    }};
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("flights", flights);

        req.getRequestDispatcher("/WEB-INF/jsp/content.jsp")
                .forward(req,resp);
    }
}
```

content.jsp:

```XML
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%@ include file="header.jsp" %>
<div>
    <span> Content. Русский </span>
    <p>Size: ${requestScope.flights.size()}</p>
    <p>id: ${requestScope.flights[1]}</p>
    <p>JSESSION ID: ${cookie["JSESSIONID"].value}, unique identifier</p>
    <p>Header: ${header["Cookie"]}</p>
    <p>Param test: ${param.test}</p>
</div>
<%@ include file="footer.jsp" %>
</body>
</html>
```

![[Untitled 65.png]]

  
---
title: Функции JSTL
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 14:04
modified: 2024-09-11T14:04:50+03:00
questions: 
notes: 
links: 
---
### Функции JSTL

Теги из этой библиотеки выполняют роль, подобную смыслу библиотеки `fmt`, только не для чисел и дат, а для строк. Теги библиотеки используют пре-

фикс `fn` и во многом копируют известные методы класса `String` и не составляют особой сложности в применении.

Подключение библиотеки осуществляется директивой `taglib`:

```XML
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
```

Список тегов:

- `${fn:length(аргумент)}` — подсчитывает число элементов в коллекции или длину строки;
- `${fn:toUpperCase(String str)}` и `${fn:toLowerCase(String str)}` — изменяет регистр строки;
- `${fn:substring(String str, int from, int to)}` — извлекает подстроку;
- `${fn:substringAfter(String str, String after)}` и `${fn:substringBefore(String str, String before)}` — извлекает подстроку до или после указанной во втором аргументе;
- `${fn:trim(String str)}` — обрезает все пробелы по краям строки;
- `${fn:replace(String str, String str1, String str2)}` — заменяет все вхождения строки str1 на строку str2;
- `${fn:split(String str, String delim)}` — разбивает строку на подстроки, используя delim, как разделитель;
- `${fn:join(массив, String delim)}` — соединяет массив в строку, вставляя между элементами подстроку delim;
- `${fn:escapeXml(String xmlString)}` — сохраняет в обрабатывемой строке теги;
- `${fn:indexOf(String str, String searchString)}` — возвращает индекс первого вхождения строки searchString;
- `<c:if test="${fn:startsWith(String str, String part)}">` — возвращает истину, если строка начинается с подстроки part;
- `<c:if test="${fn:endsWith(String str, String part)}">` — возвращает истину, если строка заканчивается на подстроку part;
- `<c:if test="${fn:contains(String name, String searchString)}">` — возвращает истину, если строка содержит подстроку searchString;
- `<c:if test="${fn:containsIgnoreCase(String name, String searchString))}">` — возвращает истину, если строка содержит подстроку searchString, без учета регистра

```Java
@WebServlet("/content")
public class ContentServlet extends HttpServlet {
    private static final FlightService flightService = FlightService.getInstance();
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("flights", flightService.findAll());

        req.getRequestDispatcher("/WEB-INF/jsp/flights.jsp")
                .forward(req,resp);


    }
}
```

flights.jsp:

```XML
<%@ page contentType="text/html;charset=UTF-8" pageEncoding="UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<html>
<head>
    <title>Title</title>
</head>
	<body>
		<h1> Список перелетов: </h1>
		<ul>
	    <c:forEach var="flight" items="${requestScope.flights}">
        <li>
						<a href="${pageContext.request.contextPath}/tickets?flightId=${flight.id}">${flight.description}</a>
				</li>
	    </c:forEach>
		</ul>
	</body>
</html>
```

==ВАЖНО!:== ==pageContext.request.contextPath== добавляет путь к application context, если мы поменяли это в конфигурациях tomcat==.==

В итоге будет ==htttp://localhost:8080==/==ourApp==/==tickets====?flightId=3==

![[Untitled 67.png]]

![[Untitled 68.png]]

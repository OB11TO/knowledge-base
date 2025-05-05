---
title: Условные выражения, циклы JSTL
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 14:03
modified: 2024-09-11T14:04:10+03:00
questions: 
notes: 
links: 
---
### Условные выражения, циклы

Общий вид `if` выражения:

```XML
<c:if test="${условие}">
Выводимый результат
</c:if?>
```

Пример `if` выражения::

```XML
<c:if test="${requestScope.user.role eq 'ADMIN'}>
<button type="submit"> DELETE </button>
</c:if>
```

Пример `if else` выражения:

```XML
<c:choose>

<c:when test="{requestScope.user.role eq 'ADMIN'}">
<span>Hey admin! </span>
</c:when>

<c:when test="{requestScope.user.role eq 'USER'}">
<span> Hello user, ${requestScope.user.name}!</span>
</c:when>

<c:otherwise>
<span> Hello anonymous!</span>
</c:otherwise>

</c:choose>
```

Пример цикла

```XML
<C:forEach var="ticket" items="${requestScope.tickets}">
<p>${ticket.seatNo}</p>
</c:forEach>

<C:forEach var="index" begin="0" end="10">
<p>${requestScope.tickets[index].seatNo}</p>  // Во коллекция должна поддерживать получение элемента по индексу, с Set такой способ не сработает
</c:forEach>
```


---
title: Ограничение применения фильтра с помощью servletNames
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 14:06
modified: 2024-09-16T14:51:50+03:00
questions: 
notes: 
links: 
---

### Ограничение применения фильтра с помощью servletNames

Также можно завязать фильтр на конкретный список названий сервлетов:

```Java
@WebServlet(value = "/register", name = "RegistrationServlet")
public class RegistrationServlet extends HttpServlet {}
```

```Java
@WebFilter(servletNames = {
        "RegistrationServlet",
        "ContentServlet"
})
public class CharsetFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding(StandardCharsets.UTF_8.name());
        response.setCharacterEncoding(StandardCharsets.UTF_8.name());
        chain.doFilter(request,response);
    }
}
```

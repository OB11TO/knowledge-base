---
title: Servlet Filters
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 14:05
modified: 2024-09-16T14:51:09+03:00
questions: 
notes: 
links: 
---

### Servlet Filters

Servlet Filters являются мощным механизмом веб-программирования в Java, предназначенным для обработки и модификации запросов и ответов веб-приложения до и после их достижения сервлетов или JSP-страниц. Они выполняют промежуточную обработку HTTP-запросов и ответов, что позволяет вам применять общие функции или преобразования для нескольких сервлетов или JSP-страниц.

![[Untitled 69.png]]

```Java
@WebFilter("/*")
public class CharsetFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding(StandardCharsets.UTF_8.name());
        response.setCharacterEncoding(StandardCharsets.UTF_8.name());
        chain.doFilter(request,response);
    }
}
```

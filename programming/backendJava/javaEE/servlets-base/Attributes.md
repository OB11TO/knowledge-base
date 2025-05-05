---
title: Attributes
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 13:59
modified: 2024-09-11T13:59:37+03:00
questions: 
notes: 
links: 
---
### Attributes

`Session` в ==основном используется, чтобы ещё хранить там какие-либо значания и ассоциировать с множеством== `request`

Если сессия будет впервые создана, то user будет равен null. Если сессия будет создана, но удалить cookie, связанный с сессией (JSESSIONID), то мы не сможем связать старую сессию, и будет создана новая, а соответственно и заново user.

```Java
import dto.UserDto;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;

@WebServlet("/sessions")
public class SessionServlet extends HttpServlet {

    private static final String USER = "user";

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        UserDto user = (UserDto) session.getAttribute(USER);
        if (user == null) {
            user = UserDto.builder()
                    .id(1L)
                    .mail("konvvadim@gmail.com")
                    .build();
            session.setAttribute(USER, user);
        }
    }
}
```

`Контекст` - ==это глобальная переменная для всех сессий и запросов.== ==методы== ==getAttributes()== ==можно вызвать на каждой цепочке схемы ниже, задать также.==

- В ==ServletContext== - будут доступны ==глобально для всего приложения==
- ==HttpSession== - Положить пользователя, чтобы ==следующие запросы ассоциировать с этим пользователем.==
- ==HttpServletRequest== (живет один запрос) - Нужен для ==jsp==

![[Untitled 54.png]]

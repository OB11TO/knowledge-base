---
title: Аутентификация, logout, авторизация Servlets
tags:
  - JavaEE
related_topics: 
created: 2024-09-11 14:07
modified: 2024-11-07T14:02:52+03:00
questions: 
notes: 
links: 
---

### Аутентификация, logout, авторизация


### аутентификация

![[Untitled 70.png]]

login.jsp:

```XML
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Login</title>
</head>
<body>
<form: action="${pageContext.request.contextPath}/login" method="post">
    <label for="email">Email:
    <input type="text" name="email" id="email" required>
</label><br>
    <label for="password">Password:
        <input type="password" name="password" id="password" required>
    </label><br>
     
    <button href="${pageContext.request.contextPath}/registration">
        <button type="button">Register</button>
    </a>
        <c:if test="${param.error != null}">
        <div style="color: red">
            <span> Error </span>
        </c:if>
</form:>
</body>
</html>
```

```Java
@WebServlet("/login")
public class LoginServlet extends HttpServlet {

    private final UserService service = UserService.getInstance();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("login")
                .forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        service.login(req.getParameter("email"), req.getPart("password"))
                .ifPresentOrElse(
                        user-> onLoginSuccess(user,req,resp),
                        ()-> onLoginFail(req,resp)
                );
    }
    @SneakyThrows
    private void onLoginFail(HttpServletRequest req, HttpServletResponse resp) {
        resp.sendRedirect("/login?error&email=" + req.getParameter("email"));
    }

    @SneakyThrows
    private void onLoginSuccess(UserDto user, HttpServletRequest req, HttpServletResponse resp){
        req.getSession().setAttribute("user", user);
        resp.sendRedirect("/flights");
    }
}
```

Дальше необходимо реализовать метод public Optional\<UserDto> login(String email, Part password).

![[Untitled 71.png]]

```Java
package mapper;

import dto.UserDto;
import entity.User;
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.factory.Mappers;

@Mapper
public interface UserMapper {

    UserMapper INSTANCE = Mappers.getMapper(UserMapper.class);

    @Mapping(target = "id", source = "user.id")
    @Mapping(target = "name", source = "user.name")
    @Mapping(target = "email", source = "user.email")
    UserDto mapToDto(User user);

    @Mapping(target = "id", ignore = true)
    @Mapping(target = "name", source = "userDto.name")
    @Mapping(target = "email", source = "userDto.email")
    User mapToEntity(UserDto userDto);
}
```

### logout

Метод invalidate() делает сессию невалидной, удаляет из ассоциативного массива, который хранится на сервере

```Java
@WebServlet("/logout")
public class LogoutServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getSession().invalidate();
        resp.sendRedirect("/login");
    }
}
```

Чтобы доступ к кнопке был в любом месте приложения, необходимо добавить ее в header.jsp, и добавлять этот header,jsp с помощью <%@ include file =”header.jsp”%> в другие jsp страницы.

```Java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<div>
    <c:if test="${not empty sessionScope.user}">
        <form action="${pageContext.request.contextPath}/logout" method="post">
            <button type ="submit">Logout </button>
        </form>
    </c:if>
</div>
```

### Авторизация

```Java
@WebFilter("/*")
public class AuthorizationFilter implements Filter {
    private static final Set<String> PUBLIC_PATH = Set.of("/login", "/registration");
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        String requestURI = ((HttpServletRequest) request).getRequestURI();
        if(isPublicPath(requestURI) || isUserLoggedIn(request)){
            chain.doFilter(request,response);
        } else{
            String prevPage = ((HttpServletRequest) request).getHeader("referer");
            ((HttpServletResponse) response).sendRedirect(prevPage!= null ? prevPage : "/login");
        }
        
    }

    private boolean isUserLoggedIn(ServletRequest request) {
        Object user = ((HttpServletRequest) request).getSession().getAttribute("user");
        UserDto userDto = (UserDto) user;
        return userDto != null;
//      return userDto != null && userDto.getRole() == Role.USER;
    }

    private boolean isPublicPath(String requestURI) {
        return PUBLIC_PATH.stream().anyMatch(requestURI::startsWith);
    }
}
```

![[Untitled 72.png]]

![[Untitled 73.png]]

![[Untitled 74.png]]

  


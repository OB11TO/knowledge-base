---
title: HTML
tags:
  - ФорматДанных
  - JavaEE
related_topics: 
created: 2024-09-11 13:52
modified: 2024-09-11T14:08:28+03:00
questions: 
notes: 
links: 
---
### HTML

Список тегов [https://html5book.ru/html-tags/](https://html5book.ru/html-tags/)

- HTML (HyperText Markup Language) - это язык разметки, используемый для создания веб-страниц.
- HTML-документ должен начинаться с объявления типа документа:

```HTML
<!DOCTYPE html>
```

- Элементы HTML описываются с помощью тегов:

```HTML
<тег>содержимое</тег>
```

- HTML-элементы могут иметь атрибуты, которые указываются внутри открывающего тега:

```HTML
<тег атрибут="значение">содержимое</тег>
```

- Вложенные элементы должны быть правильно вложены друг в друга:

```HTML
<div>
  <p>Абзац</p>
</div>
```

- Специальные символы, такие как `<`, `>`, `&`, должны быть заменены соответствующими символами-сущностями:

```HTML
<p>5 &gt; 3</p>
```

Это основные правила для написания данных в форматах XML, JSON, YAML и HTML. Надеюсь, это поможет вам в работе с этими форматами!

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="path/to/script.js"></script>
    <link rel="stylesheet" href="path/to/simple.css">
</head>
<body>
<div>

</div>
<h1>
    Hello world!
</h1>
<div>
    <form>
        <label>
            Username:
            <input type="text" method="post">
        </label>
        <label>
            Password:
            <input type="password" name="username">
        </label>
        <button type="submit">SEND</button>
    </form>
</div>
<span></span>

</body>
</html>
```

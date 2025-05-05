---
title: Принципы Kiss, Dry и Yagni
tags:
  - Pattern
  - Architecture
related_topics: 
created: 2025-02-13 14:05
modified: 2025-02-13T14:38:17+03:00
questions: 
notes: 
links: 
---


------
[[Принципы Kiss, Dry и Yagni подробнее]]



------



**KISS** - <mark class="hltr-red">keep it simple and shor</mark>t. <mark class="hltr-green2">Код должен быть простым и коротким.</mark>

```java
package ru.job4j.condition;

public class Maximum {
    public static int getMax(int first, int second, int third, int forth) {
        int result = forth;
        if (first > second) {
            if (first > third) {
                if (first > forth) {
                    result = third;
                } 
            }
        } else if (second > third) {
            if (second > forth) {
                result = first;
            }
        } else if (third > forth) {
            result = second;
        } 
        return result;
    }
}
```

В этом коде есть ошибка. Искать ее было сложно, потому что код написан сложно и его много
```java
package ru.job4j.condition;

public class Maximum {
    public static int getMax(int left, int right) {
        return left > right ? left : right;
    }

    public static int max(int first, int second, int third, int forth) {
        return getMax(
                getMax(first, second),
                getMax(third, forth)
        );
    }
}
```


----

**DRY** - <mark class="hltr-red">don't repeat yourselft.</mark> Дословно <mark class="hltr-green2">"не повторяй себя".</mark> Противоположность этому принципу - copypaste. То есть, <mark class="hltr-yellow">старайтесь использовать уже существующие методы, чтобы решить новую задачу. Не копируйте код.</mark>
Дублирование кода – пустая трата времени и ресурсов. Вам придется поддерживать одну и ту же логику и тестировать код сразу в двух местах, причем если вы измените код в одном месте, его нужно будет изменить и в другом.

-----

**YAGNI** -<mark class="hltr-red"> You aren't gonna need it.</mark> Принцип пересекается со втором. <mark class="hltr-green2">Подумайте, стоит ли создавать новый метод. Можно ли решить задачу уже существующими методами.</mark>

```java
package ru.job4j.lambda;

import java.io.File;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Pattern;

public class Search {
    static List<File> findByMask(String mask) {
        List<File> result = new ArrayList<>();
        for (File file : ..) {
            if (Pattern.matches(mask, file.getName())) {
                rsl.add(file);
            }
        }
        return result;
    }

    static List<File> findByName(String name) {
        List<File> result = new ArrayList<>();
        for (File file : ..) {
            if (name.equals(file.getName())) {
                result.add(file);
            }
        }
        return result;
    }

    static List<File> findByExtension(String extension) {
        List<File> result = new ArrayList<>();
        for (File file : ..) {
            if (file.getName().endWiths(extension)) {
                result.add(file);
            }
        }
        return result;
    }
}
```

Здесь можно увидеть копирование кода. Все методы повторяют друг друга и отличаются только видом проверки.

```java
package ru.job4j.lambda;

import java.io.File;
import java.util.ArrayList;
import java.util.List;
import java.util.function.Predicate;
import java.util.regex.Pattern;

public class Search {

    static List<File> findBy(Predicate<File> predicate) {
        List<File> result = new ArrayList<>();
        for (File file : ..) {
            if (predicate.test(file)) {
                result.add(file);
            }
        }
        return result;
    }
    static List<File> findByMask(String mask) {
        return findBy(file -> Pattern.matches(mask, file.getName()));
    }

    static List<File> findByName(String name) {
        return findBy(file -> file.getName().equals(name));
    }

    static List<File> findByExt(String extension) {
        return findBy(file -> file.getName().endsWith(extension));
    }
}
```


-----

**BDUF**  <mark class="hltr-red">Big Design Up Front</mark>
<mark class="hltr-green2">_Глобальное проектирование прежде всего_</mark>

Этот подход к разработке программного обеспечения очень важен, и его часто игнорируют. <mark class="hltr-yellow">Прежде чем переходить к реализации, убедитесь, что все хорошо продумано.</mark>


------

**APO** <mark class="hltr-red">Avoid Premature Optimization</mark>
<mark class="hltr-green2">_Избегайте преждевременной оптимизации_</mark>

Эта практика побуждает разработчиков оптимизировать код до того, как необходимость этой оптимизации будет доказана. Думаю, что если вы следуете KISS или YAGNI, вы не попадетесь на этот крючок.  
  
Поймите правильно, предвидеть, что произойдет что-то плохое – это хорошо. Но прежде чем вы погрузитесь в детали реализации, убедитесь, что эти оптимизации действительно полезны.  
  
Очень простой пример – масштабирование. Вы не станете покупать 40 серверов из предположения, что ваше новое приложение станет очень популярным. Вы будете добавлять серверы по мере необходимости.  
  
<mark class="hltr-yellow">Преждевременная оптимизация может привести к задержкам в коде и, следовательно, увеличит затраты времени на вывод функций на рынок</mark>.  
  
Многие считают преждевременную оптимизацию корнем всех зол.

-----

**Бри́тва О́ккама**
<mark class="hltr-green2">Не следует привлекать новые сущности без крайней на то необходимости</mark>
Не создавайте ненужных сущностей без необходимости. Б<mark class="hltr-yellow">удьте прагматичны — подумайте, нужны ли они, поскольку они могут в конечном итоге усложнить вашу кодовую базу.</mark>

-----


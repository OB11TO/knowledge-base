---
title: Компактные строки Java
tags:
  - JavaSE
  - String
related_topics: 
created: 2024-09-02 17:59
modified: 2024-09-04T12:54:15+03:00
difficulty: 
questions: 
notes: 
links:
  - https://topjava.ru/blog/compact-strings-java-9
---
# Компактные строки  в Java 9
## <mark class="hltr-orange">Обзор</mark>

Наверно, ни для кого не является секретом, что <mark class="hltr-yellow">строки в Java представлены в виде массива символов **char[]**</mark>. При этом каждый <mark class="hltr-green2">символ в памяти занимает 2 байта (16 бит), т.к. Java использует кодировку **UTF-16**.  </mark>
Например, если строка содержит слово <mark class="hltr-yellow">на английском языке, то 8 первых бит у каждого символа будут равны 0, поскольку символ **ASCII** может быть представлен одним байтом вместо двух.  </mark>
Многим символам необходимо 16 бит для их представления, но по статистике для большинства данных требуется только 8 бит, представленных символами **LATIN-1**. Исходя из этого можно попробовать улучшить потребление памяти и производительность.  
Также важно то, что<mark class="hltr-yellow"> строки обычно занимают большую часть пространства кучи JVM</mark>. И, как сказано выше, в большинстве случаев они могут занимать места в два раза больше, чем им в действительности необходимо.  

В этой статье мы обсудим опцию **Compressed String** (сжатая строка), представленную в JDK 6, и новую **Compact String** (компактную строку), появившуюся в JDK 9. Обе опции были разработаны для оптимизации потребления памяти строками в JVM.

## **<mark class="hltr-orange">Сжатие строк в Java 6</mark>**

В JDK 6 в 21 обновлении была представлена новая опция для JVM:

```java
-XX: + UseCompressedStrings
```

<mark class="hltr-purple">Когда эта опция включена, строки хранятся не как char[], а как byte[]</mark>, что экономит много памяти. <mark class="hltr-red">Однако эта опция была в конечном итоге удалена в JDK 7 из-за непредсказуемых последствий для производительности.</mark>

##  <mark class="hltr-orange">Компактные строки в Java 9</mark>

<mark class="hltr-red">В Java 9 вернули концепцию компактных строк.  
</mark>
Это означает, что всякий раз, когда мы создаем строку символы которой могут быть представлены с использованием одного байта – в LATIN-1, то для хранения строк будет использоваться байтовый массив. Но,<mark class="hltr-purple"> если какой-либо символ требует более 8 бит для своего представления, то каждый символы сроки будет занимать два байта</mark> (UTF-16).  

>[!faq]- [Теперь вопрос — как будут работать все операции со строками?<mark class="hltr-yellow"> Как будут различаться кодировки строк? </mark> 
> Для решения этой проблемы было внесено ещё одно изменение во внутреннюю реализацию **String**. <mark class="hltr-yellow">Теперь данный класс содержит поле </mark>**private final byte coder**, которое хранит эту информацию.  

###  <mark class="hltr-orange">Реализация строк в Java 9</mark>

До сих пор строка хранилась, как массив символов **char[]**:

```java
private final char[] value;
```

Теперь это массив байт **byte[]**:

```java
private final byte[] value;
```

Идентификатор, отвечающий за кодировку **coder**:

```java
private final byte coder;
```

При этом идентификатор поддерживает следующие значения:

```java
static final byte LATIN1 = 0;
static final byte UTF16 = 1;
```

Большинство методов класса **String** теперь проверяют поле **coder** и в зависимости от его значения использую разную реализацию:

```java
public int indexOf(int ch, int fromIndex) {
    return isLatin1()
            ? StringLatin1.indexOf(value, ch, fromIndex)
            : StringUTF16.indexOf(value, ch, fromIndex);
} 
 
private boolean isLatin1() {
    return COMPACT_STRINGS && coder == LATIN1;
}
```

<mark class="hltr-yellow">Для отключения компактных строк существует опция:</mark>

```java
+XX:-CompactStrings
```

### <mark class="hltr-orange">Как работает coder</mark>

В реализации класса **String** в Java 9 длина вычисляется так:

```java
public int length () {
     return value.length >> coder;
}
```

Если <mark class="hltr-yellow">строка содержит только</mark> **LATIN-1**, <mark class="hltr-yellow">значение кодировщика будет равно 0</mark>, поэтому <mark class="hltr-red">длина строки будет равна длине байтового массива.  </mark>

Если строка представлена в виде **UTF-16**, то значение кодировщика будет равно 1, и, следовательно, длина будет вдвое меньше размера фактического байтового массива.  

###  <mark class="hltr-orange">Compact Strings vs. Compressed String</mark>

В **<mark class="hltr-red">Compressed Strings</mark>** в JDK 6 основной проблемой было то, что конструктор **String** принимал в качестве аргумента только массив символов **char[]** не смотря на то, что многие операции со **String** зависят от представления **char[]**, а не от байтового массива. Из-за этого <mark class="hltr-yellow">приходилось производить распаковку, что сказывалось на производительности.  </mark>

Обратите внимание, что в случае<mark class="hltr-red"> **Compact String** </mark>содержание дополнительного поля **coder** также может увеличить нагрузку. <mark class="hltr-yellow">Чтобы снизить</mark> «стоимость» кодера и распаковку байтов в символы (в случае представления **UTF-16**), <mark class="hltr-yellow">некоторые методы являются встроенными, и код ASM, сгенерированный компилятором JIT, также был улучшен.  </mark>

Эти изменение привели к некоторым неожиданным результатам. **LATIN-1 indexOf(String)** вызывает встроенный метод, тогда как **indexOf(char)** — нет. В случае **UTF-16** оба эти метода вызывают встроенный метод. Эта проблема касается только строки **LATIN-1** и будет исправлена в будущих выпусках.  

Таким образом, с точки зрения производительности, компактные строки **Compact Strings** лучше, чем сжатые строки **Compressed Strings**.  

Чтобы узнать, сколько памяти сохранено с помощью **Compact Strings**, были проанализированы различные дампы кучи (heap) Java-приложений. И, хотя результаты сильно зависели от конкретных приложений, общие улучшения были почти всегда значительными.

### <mark class="hltr-orange">Различия в производительности</mark>

Давайте рассмотрим очень простой пример, демонстрирующий разницу в производительности между включенным и отключенным **Compact Strings**:

```java
package ru.topjava;

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class CompactStringTest {

    public static void main(String[] args) {
        long startTime = System.currentTimeMillis();
        List<String> strings = IntStream.rangeClosed(1, 100_000)
                .mapToObj(Integer::toString)
                .collect(Collectors.toList());

        long totalTime = System.currentTimeMillis() - startTime;
        System.out.println("Generated " + strings.size() + " strings in " + totalTime + " ms.");

        startTime = System.currentTimeMillis();
        String appended = strings.stream()
                .reduce("", (l, r) -> l + r);

        totalTime = System.currentTimeMillis() - startTime;
        System.out.println("Created string of length " + appended.length() + " in " + totalTime + " ms.");
    }
}
```

Когда мы запускаем этот код (**Compact Strings** <mark class="hltr-green2">включены по умолчанию)</mark>, мы получаем вывод:

![](https://optim.tildacdn.com/tild6665-3037-4931-b134-653538393833/-/resize/760x/-/format/webp/carbon_7.png)

Точно так же, если мы запустим, <mark class="hltr-green2">отключив</mark> **Compact Strings** с помощью опции:

```
-XX: -CompactStrings
```

Получим:

![](https://optim.tildacdn.com/tild3364-6130-4630-b936-623137383562/-/resize/760x/-/format/webp/carbon_8.png)

Понятно, что это тест поверхностен, и он не может быть очень репрезентативным — это всего лишь пример того, как новая опция может улучшить производительность в этом конкретном сценарии.
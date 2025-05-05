---
title: FilterInputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:50
modified: 2025-01-14T12:55:17+03:00
questions: 
notes: 
links: 
---

### 6. **`FilterInputStream`**

- **Описание**: Базовый класс для потоков, которые <mark class="hltr-red">декорируют другие потоки для добавления дополнительных функциональностей</mark> (например, буферизация, сжатие).
- **Особенности**:
    - <mark class="hltr-yellow">Все классы, которые оборачивают другие потоки</mark> (например, `BufferedInputStream`, `DataInputStream`), наследуются от `FilterInputStream`.
    - Этот класс сам по себе редко используется напрямую, но предоставляет основу для потоков-декораторов. [[Декоратор]]
    - <mark class="hltr-yellow">Цель</mark> этого класса — <mark class="hltr-yellow">расширить возможности стандартных потоков ввода</mark> (например, добавить буферизацию, обработку данных и т.п.) без изменения их внутренней логики.

##### Пример использования `FilterInputStream`

Допустим, мы хотим прочитать файл и при этом добавить буферизацию. В этом случае мы можем обернуть `FileInputStream` в `BufferedInputStream`, что улучшит производительность при чтении больших файлов.

##### Пример без декоратора:
```java
import java.io.FileInputStream;
import java.io.IOException;

public class SimpleReadExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("file.txt")) {
            int data;
            while ((data = fis.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```
Этот код читает файл байт за байтом, но каждый вызов метода `read()` обращается к диску, что делает его неэффективным при работе с большими объемами данных.

###### Пример с использованием декоратора (`BufferedInputStream`):

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class BufferedReadExample {
    public static void main(String[] args) {
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("file.txt"))) {
            int data;
            while ((data = bis.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

В этом примере:

- `BufferedInputStream` оборачивает `FileInputStream`.
- Теперь данные читаются буферами, что уменьшает количество обращений к диску, повышая производительность.

##### Как работает `FilterInputStream`?

`FilterInputStream` сам по себе не изменяет поведение потоков ввода. Он делегирует вызовы методов, таких как `read()`, внутреннему потоку (`in`), который был передан в его конструктор.

##### Пример создания собственного декоратора:

Допустим, мы хотим создать поток, который подсчитывает количество прочитанных байтов.
```java
import java.io.FilterInputStream;
import java.io.IOException;
import java.io.InputStream;

class CountingInputStream extends FilterInputStream {
    private int bytesRead = 0;

    protected CountingInputStream(InputStream in) {
        super(in);
    }

    @Override
    public int read() throws IOException {
        int data = super.read();
        if (data != -1) {
            bytesRead++;
        }
        return data;
    }

    @Override
    public int read(byte[] b, int off, int len) throws IOException {
        int result = super.read(b, off, len);
        if (result != -1) {
            bytesRead += result;
        }
        return result;
    }

    public int getBytesRead() {
        return bytesRead;
    }
}

public class Main {
    public static void main(String[] args) {
        try (CountingInputStream cis = new CountingInputStream(new FileInputStream("file.txt"))) {
            while (cis.read() != -1) {
                // Читаем данные
            }
            System.out.println("Прочитано байтов: " + cis.getBytesRead());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

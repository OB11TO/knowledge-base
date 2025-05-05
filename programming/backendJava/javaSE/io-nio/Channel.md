---
title: Channel
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:02
modified: 2024-09-12T16:36:06+03:00
questions: 
notes: 
links: 
---

----
##### Подклассы Channel 
[[FileChannel]]
[[SocketChannel]]
[[ServerSocketChannel]]
[[DatagramChannel]]
[[Pipe.SinkChannel и Pipe.SourceChannel]]
[[AsynchronousFileChannel]]

-----

### Channel

Класс `Channel` в пакете `java.nio.channels` <mark class="hltr-red">представляет канал, который обеспечивает двустороннюю связь между источником данных и приемником данных</mark>. Каналы <mark class="hltr-yellow">используются для чтения и записи данных из и в буферы.</mark>

Ниже приведены основные методы и свойства класса `Channel`:

- `isOpen()`: Возвращает `true`, если канал открыт, и `false`, если закрыт.
- `close()`: Закрывает канал.
- `read(ByteBuffer buffer)`: <mark class="hltr-purple">Читает данные из канала в указанный буфер.</mark>
- `write(ByteBuffer buffer)`: <mark class="hltr-purple">Записывает данные из указанного буфера в канал.</mark>
- `transferTo(long position, long count, WritableByteChannel target)`: Передает данные из текущего канала в указанный канал, начиная с указанной позиции и с заданным количеством байтов.
- `transferFrom(ReadableByteChannel src, long position, long count)`: Передает данные из указанного канала в текущий канал, начиная с указанной позиции и с заданным количеством байтов.

Пример использования класса `FileChannel` для чтения и записи данных:

```Java
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class ChannelExample {
    public static void main(String[] args) {
        Path path = Paths.get("example.txt");
        String data = "Hello, Channel!";

        // Запись данных в файл с использованием канала
        try (FileChannel channel = FileChannel.open(path, StandardOpenOption.WRITE, StandardOpenOption.CREATE)) {
            ByteBuffer buffer = ByteBuffer.allocate(data.length());
            buffer.put(data.getBytes());  //записали в буффер данные 
            buffer.flip();  //он находится в режиме записи, нужно подготвоить режим на чтение  
            channel.write(buffer); //читаем из буффера и записываем в файл
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Чтение данных из файла с использованием канала
        try (FileChannel channel = FileChannel.open(path, StandardOpenOption.READ)) {
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            int bytesRead = channel.read(buffer);  //сразу читаем из файла и записываем в буффер
            buffer.flip(); 
            byte[] readData = new byte[bytesRead]; //Создается массив байтов `readData`, размер которого равен количеству прочитанных байтов `bytesRead`.
            buffer.get(readData); //Метод `get()` читает данные из буфера и записывает их в массив `readData` После этого позиция буфера снова сдвигается вперед по мере чтения данных.
            System.out.println(new String(readData));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
- Создается новый байтовый буфер `ByteBuffer` с размером, равным длине строки `data`. Этот буфер будет использоваться для хранения байтов перед их записью в файл.
- Метод `allocate()` выделяет пространство в памяти для байтового буфера, чтобы он мог хранить данные перед их записью в файл.
- Данные строки `data` конвертируются в массив байтов с помощью метода `getBytes()` <mark class="hltr-yellow">и записываются в буфер</mark> `buffer`.
- В этот момент буфер находится в **режиме записи**, то есть в него можно добавлять байты.
- Метод `flip()` переключает буфер в **режим чтения**.
    - После вызова `put()` буфер находится в режиме записи, где позиция указывает на конец данных. Чтобы прочитать данные из буфера и передать их в канал для записи в файл, необходимо переключить буфер в режим чтения.
    - `flip()` устанавливает текущую позицию на начало данных, ограничивая количество байтов, которые можно прочитать (от текущей позиции до лимита).
- Теперь, когда буфер находится в режиме чтения, вызывается метод `write()` на канале `FileChannel`. Он записывает содержимое буфера (данные из строки `data`) в файл по указанному пути `path`.
- При этом происходит чтение данных из буфера начиная с текущей позиции до лимита.


----

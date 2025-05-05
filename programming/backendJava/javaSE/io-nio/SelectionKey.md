---
title: SelectionKey
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:17
modified: 2024-09-10T18:19:24+03:00
questions: 
notes: 
links: 
---
### SelectionKey

Класс `SelectionKey` в пакете `java.nio.channels` представляет ключ, связанный с регистрацией канала на селекторе (`Selector`). Этот ключ предоставляет информацию о готовности канала для определенных операций ввода-вывода, таких как чтение, запись, принятие соединений или подключение.

#### Основные типы операций `SelectionKey`:

- **SelectionKey.OP_CONNECT** — канал, который готов к подключению к серверу.
- **SelectionKey.OP_ACCEPT** — канал, который готов принимать входящие соединения.
- **SelectionKey.OP_READ** — канал, который готов к чтению данных.
- **SelectionKey.OP_WRITE** — канал, который готов к записи данных.

#### Методы класса `SelectionKey`:

- **isValid()**: Проверяет, является ли ключ действительным. Если ключ недействителен, это означает, что он был отменен или его канал был закрыт.
    
- **channel()**: Возвращает канал, связанный с данным ключом. Это позволяет получить доступ к каналу, зарегистрированному на селекторе.
    
- **selector()**: Возвращает селектор, к которому данный ключ относится. Это может быть полезно для получения селектора, если нужно выполнить дополнительные операции с ним.
    
- **interestOps()**: Возвращает набор операций, для которых данный ключ зарегистрирован. Это позволяет узнать, какие операции интересуют данный канал.
    
- **interestOps(int ops)**: Задает новый набор операций для данного ключа. Этот метод используется для обновления набора интересующих операций.
    
- **readyOps()**: Возвращает набор готовых операций для данного ключа. Это позволяет узнать, какие из зарегистрированных операций готовы к выполнению.
    
- **isReadable()**: Проверяет, готов ли канал для операции чтения. Возвращает `true`, если канал готов к чтению данных.
    
- **isWritable()**: Проверяет, готов ли канал для операции записи. Возвращает `true`, если канал готов к записи данных.
    
- **isAcceptable()**: Проверяет, готов ли канал для операции принятия соединения. Возвращает `true`, если канал готов принять входящее соединение.
    
- **isConnectable()**: Проверяет, готов ли канал для операции установки соединения. Возвращает `true`, если канал готов завершить процесс подключения.
    
- **attach(Object obj)**: Присоединяет объект к данному ключу. Этот метод позволяет прикрепить произвольный объект (например, пользовательские данные) к ключу для дальнейшего использования.
    
- **attachment()**: Возвращает объект, присоединенный к данному ключу. Это может быть полезно для получения дополнительной информации, связанной с ключом.
    

#### Пример использования `SelectionKey`:

```java
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.net.InetSocketAddress;

public class SelectorExample {
    public static void main(String[] args) throws IOException {
        // Открываем серверный сокетный канал
        ServerSocketChannel serverChannel = ServerSocketChannel.open();
        serverChannel.bind(new InetSocketAddress(8080));
        serverChannel.configureBlocking(false);

        // Открываем селектор
        Selector selector = Selector.open();

        // Регистрируем серверный канал на селекторе
        serverChannel.register(selector, SelectionKey.OP_ACCEPT);

        while (true) {
            // Ожидание готовности каналов
            selector.select();

            // Получаем выбранные ключи
            for (SelectionKey key : selector.selectedKeys()) {
                selector.selectedKeys().remove(key);

                if (!key.isValid()) {
                    continue;
                }

                if (key.isAcceptable()) {
                    // Принимаем входящее соединение
                    ServerSocketChannel server = (ServerSocketChannel) key.channel();
                    SocketChannel clientChannel = server.accept();
                    clientChannel.configureBlocking(false);
                    clientChannel.register(selector, SelectionKey.OP_READ);
                } else if (key.isReadable()) {
                    // Чтение данных из канала
                    SocketChannel clientChannel = (SocketChannel) key.channel();
                    ByteBuffer buffer = ByteBuffer.allocate(256);
                    int numRead = clientChannel.read(buffer);
                    if (numRead == -1) {
                        clientChannel.close();
                    } else {
                        String receivedData = new String(buffer.array()).trim();
                        System.out.println("Received: " + receivedData);
                    }
                }
            }
        }
    }
}

```

Использование `SelectionKey` в сочетании с `Selector` позволяет эффективно управлять многими каналами ввода-вывода в одном потоке, что значительно улучшает производительность и масштабируемость сетевых приложений.
```java
try {
    // Создание Selector
    Selector selector = Selector.open();

    // Регистрация канала на Selector
    SocketChannel channel = SocketChannel.open();
    channel.configureBlocking(false);
    SelectionKey key = channel.register(selector, SelectionKey.OP_READ);

    // Проверка готовности канала для операций
    if (key.isReadable()) {
        // Выполнение операции чтения
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        int bytesRead = channel.read(buffer);

        // Обработка прочитанных данных

        buffer.clear();
    }

    // Присоединение объекта к ключу
    String attachment = "Some data";
    key.attach(attachment);

    // Получение прикрепленного объекта
    String attachedData = (String) key.attachment();
} catch (IOException e) {
    e.printStackTrace();
}
```

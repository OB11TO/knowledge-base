---
title: Selector
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:14
modified: 2024-09-12T16:45:09+03:00
questions: 
notes: 
links: 
---
### Selector

Класс `Selector` в пакете `java.nio.channels` предоставляет <mark class="hltr-yellow">механизм для мультиплексирования операций ввода-вывода на неблокирующих каналах</mark>. Это позволяет приложениям эффективно обрабатывать несколько каналов ввода-вывода с использованием одного потока, что особенно полезно для создания масштабируемых серверов и клиентов.

Основная идея работы `Selector` <mark class="hltr-green2">заключается в регистрации каналов и определении их готовности для операций ввода-вывода.</mark> Метод `select()` б<mark class="hltr-red">локирует поток до тех пор, пока один или несколько каналов не будут готовы для обработки.
</mark>
#### Основные методы класса `Selector`:

- **open()**: Создает новый объект `Selector`. Этот статический метод возвращает новый экземпляр `Selector`.
    
- **isOpen()**: Проверяет, открыт ли `Selector`.
    
- **select()**: Блокирует вызывающий поток до тех пор, пока один или несколько каналов не станут готовыми для операций ввода-вывода. Возвращает количество готовых каналов.
    
- **select(long timeout)**: Блокирует вызывающий поток и ожидает готовности каналов в течение указанного времени (в миллисекундах). Возвращает количество готовых каналов.
    
- **selectNow()**: Немедленно проверяет готовность каналов для операций ввода-вывода без блокировки потока. Возвращает количество готовых каналов.
    
- **wakeup()**: Разблокирует поток, который находится в методе `select()` или `select(long timeout)`. Это позволяет прекратить ожидание и немедленно продолжить выполнение.
    
- **keys()**: Возвращает набор ключей, зарегистрированных в `Selector`. Каждый ключ представляет собой зарегистрированный канал и его интересующие операции.
    
- **selectedKeys()**: Возвращает набор ключей, которые были выбраны в результате последнего вызова метода `select()`, `select(long timeout)` или `selectNow()`. Эти ключи соответствуют каналам, готовым для операций ввода-вывода.
    
- **close()**: Закрывает `Selector` и освобождает все связанные с ним ресурсы.
    

#### Пример использования `Selector`:

```java
import java.io.IOException;
import java.nio.channels.*;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;

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

                if (key.isAcceptable()) {
                    // Принимаем входящее соединение
                    SocketChannel clientChannel = serverChannel.accept();
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



```java
try {
    // Создание Selector
    Selector selector = Selector.open();

    // Регистрация канала на Selector
    SocketChannel channel = SocketChannel.open();
    channel.configureBlocking(false);
    channel.register(selector, SelectionKey.OP_READ);

    // Бесконечный цикл обработки готовых каналов
    while (true) {
        // Ожидание готовности каналов
        int readyChannels = selector.select();

        if (readyChannels == 0) {
            continue;
        }

        // Получение выбранных ключей
        Set<SelectionKey> selectedKeys = selector.selectedKeys();

        // Обработка выбранных ключей
        for (SelectionKey key : selectedKeys) {
            if (key.isReadable()) {
                // Чтение данных с канала
                SocketChannel socketChannel = (SocketChannel) key.channel();
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                int bytesRead = socketChannel.read(buffer);

                // Обработка прочитанных данных

                buffer.clear();
            }
        }

        // Очистка выбранных ключей
        selectedKeys.clear();
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

В данном примере мы создаем `Selector`, регистрируем неблокирующий `SocketChannel` на нем и затем в бесконечном цикле ожидаем готовности каналов для операций ввода-вывода. Когда канал становится готовым для чтения (`isReadable()`), мы считываем данные с канала и обрабатываем их.

### Заключение

Использование `Selector` позволяет эффективно обрабатывать несколько каналов ввода-вывода в одном потоке, что особенно полезно для создания высокопроизводительных серверов и клиентов. Это позволяет значительно уменьшить количество потоков и улучшить масштабируемость приложений.
---
title: AsynchronousFileChannel
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:32
modified: 2024-09-11T11:33:02+03:00
questions: 
notes: 
links: 
---
### AsynchronousFileChannel

Класс `AsynchronousFileChannel` в пакете `java.nio.channels` представляет асинхронный канал для работы с файлами. Это позволяет выполнять асинхронные операции ввода-вывода, такие как чтение и запись данных, не блокируя основной поток выполнения.

#### Основные методы и функциональности `AsynchronousFileChannel`:

- **open(Path path, Set\<OpenOption> options, ExecutorService executor, FileAttribute\<?> .. attrs)**: Создает новый `AsynchronousFileChannel` для заданного пути к файлу с определенными опциями и атрибутами. Метод `open()` является статическим и возвращает новый экземпляр `AsynchronousFileChannel`.
    
- **isOpen()**: Проверяет, открыт ли `AsynchronousFileChannel`. Возвращает `true`, если канал открыт, и `false`, если закрыт.
    
- **read(ByteBuffer dst, long position, Object attachment, CompletionHandler<Integer, ? super Object> handler)**: Асинхронно читает данные из файла в заданный буфер (`ByteBuffer`) начиная с указанной позиции. Метод принимает `CompletionHandler`, который будет вызван при завершении операции.
    
- **write(ByteBuffer src, long position, Object attachment, CompletionHandler<Integer, ? super Object> handler)**: Асинхронно записывает данные из заданного буфера (`ByteBuffer`) в файл начиная с указанной позиции. Метод принимает `CompletionHandler`, который будет вызван при завершении операции.
    
- **close()**: Закрывает `AsynchronousFileChannel`.
    
- **lock(long position, long size, boolean shared, Object attachment, CompletionHandler<FileLock, ? super Object> handler)**: Асинхронно блокирует область файла, начиная с указанной позиции и с определенным размером. Метод принимает `CompletionHandler`, который будет вызван при завершении операции блокировки.
    
- **tryLock(long position, long size, boolean shared)**: Пытается асинхронно установить блокировку на область файла, начиная с указанной позиции и с определенным размером. Возвращает объект `FileLock` если блокировка успешно установлена, или `null` если нет.
    

#### Пример использования `AsynchronousFileChannel`:
```java
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.AsynchronousFileChannel;
import java.nio.channels.CompletionHandler;
import java.nio.file.Paths;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class AsynchronousFileChannelExample {
    public static void main(String[] args) throws IOException {
        ExecutorService executor = Executors.newFixedThreadPool(1);
        AsynchronousFileChannel fileChannel = AsynchronousFileChannel.open(
                Paths.get("example.txt"),
                java.nio.file.StandardOpenOption.CREATE,
                java.nio.file.StandardOpenOption.WRITE,
                java.nio.file.StandardOpenOption.READ,
                executor
        );
        
        // Запись данных в файл
        ByteBuffer writeBuffer = ByteBuffer.allocate(1024);
        writeBuffer.clear();
        writeBuffer.put("Hello, Asynchronous FileChannel!".getBytes());
        writeBuffer.flip();
        
        fileChannel.write(writeBuffer, 0, null, new CompletionHandler<Integer, Void>() {
            @Override
            public void completed(Integer result, Void attachment) {
                System.out.println("Write completed with " + result + " bytes written.");
                try {
                    fileChannel.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            @Override
            public void failed(Throwable exc, Void attachment) {
                exc.printStackTrace();
            }
        });
        
        // Чтение данных из файла
        ByteBuffer readBuffer = ByteBuffer.allocate(1024);
        fileChannel.read(readBuffer, 0, null, new CompletionHandler<Integer, Void>() {
            @Override
            public void completed(Integer result, Void attachment) {
                readBuffer.flip();
                while (readBuffer.hasRemaining()) {
                    System.out.print((char) readBuffer.get());
                }
                System.out.println();
            }

            @Override
            public void failed(Throwable exc, Void attachment) {
                exc.printStackTrace();
            }
        });
    }
}

```

### Сравнение и использование

- **`Pipe.SinkChannel` и `Pipe.SourceChannel`**: Эти каналы предназначены для обмена данными между потоками в одном процессе. Это удобное решение для межпотокового общения, когда данные не выходят за пределы одного процесса. Они не предназначены для межпроцессного общения и не могут быть использованы для сетевых операций.
    
- **`AsynchronousFileChannel`**: Предназначен для асинхронного чтения и записи файлов. Это полезно для приложений, которые требуют высокой производительности и не хотят блокировать основной поток выполнения при выполнении операций ввода-вывода. Он поддерживает асинхронные операции, что позволяет эффективно обрабатывать большие объемы данных и улучшать отзывчивость приложений.
    

Выбор между `Pipe.SinkChannel`/`Pipe.SourceChannel` и `AsynchronousFileChannel` зависит от конкретных потребностей вашего приложения: либо для межпотокового общения внутри одного процесса, либо для асинхронного ввода-вывода с файлами.
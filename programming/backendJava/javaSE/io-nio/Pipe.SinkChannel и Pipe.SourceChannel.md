---
title: Pipe.SinkChannel и Pipe.SourceChannel
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:32
modified: 2024-09-11T11:32:44+03:00
questions: 
notes: 
links: 
---
### Pipe.SinkChannel и Pipe.SourceChannel

Классы `Pipe.SinkChannel` и `Pipe.SourceChannel` в пакете `java.nio.channels` предоставляют возможность обмена данными между двумя потоками внутри одного процесса через канал.

#### Основные методы и функциональности `Pipe.SinkChannel` и `Pipe.SourceChannel`:

- **Pipe.SinkChannel**:
    
    - **write(ByteBuffer src)**: Записывает данные из указанного буфера (`ByteBuffer`) в канал. Этот метод блокирует выполнение до тех пор, пока все данные не будут записаны.
    - **close()**: Закрывает канал, освобождая все связанные ресурсы.
- **Pipe.SourceChannel**:
    
    - **read(ByteBuffer dst)**: Читает данные из канала в указанный буфер (`ByteBuffer`). Этот метод блокирует выполнение до тех пор, пока данные не будут прочитаны.
    - **close()**: Закрывает канал, освобождая все связанные ресурсы.

#### Пример использования `Pipe.SinkChannel` и `Pipe.SourceChannel`:
```java
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.Pipe;

public class PipeExample {
    public static void main(String[] args) throws IOException {
        // Создание нового Pipe
        Pipe pipe = Pipe.open();
        
        // Получение SinkChannel и SourceChannel
        Pipe.SinkChannel sinkChannel = pipe.sink();
        Pipe.SourceChannel sourceChannel = pipe.source();
        
        // Запись данных в SinkChannel в одном потоке
        new Thread(() -> {
            try {
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                buffer.clear();
                buffer.put("Hello, Pipe!".getBytes());
                buffer.flip();
                while (buffer.hasRemaining()) {
                    sinkChannel.write(buffer);
                }
                sinkChannel.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }).start();
        
        // Чтение данных из SourceChannel в другом потоке
        new Thread(() -> {
            try {
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                int bytesRead;
                while ((bytesRead = sourceChannel.read(buffer)) != -1) {
                    buffer.flip();
                    while (buffer.hasRemaining()) {
                        System.out.print((char) buffer.get());
                    }
                    buffer.clear();
                }
                sourceChannel.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }).start();
    }
}

```


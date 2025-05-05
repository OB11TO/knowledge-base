---
title: FilterOutputStream
tags:
  - IO-NIO
related_topics: 
created: 2024-09-10 18:55
modified: 2024-09-10T18:55:56+03:00
questions: 
notes: 
links: 
---
### 6. **`FilterOutputStream`**

- **Описание**: Базовый класс для потоков, которые добавляют дополнительную функциональность к потокам вывода (<mark class="hltr-red">декораторы</mark>).
- **Особенности**:
    - Не используется напрямую, но является основой для классов-декораторов, таких как `BufferedOutputStream` и `DataOutputStream`.
- **Пример**:
    - `BufferedOutputStream` и `DataOutputStream` являются примерами классов, которые наследуются от `FilterOutputStream`.
- Можно создать СВОЮ Реализацию 

```java
import java.io.FileOutputStream;
    import java.io.FilterOutputStream;
    import java.io.IOException;
    import java.io.OutputStream;
    
    public class FilterOutputStreamExample {
        public static void main(String[] args) {
            try {
                // Создаем файловый поток вывода
                FileOutputStream fos = new FileOutputStream("output.txt");
    
                // Создаем фильтрующий поток вывода с FilterOutputStream
                CustomFilterOutputStream filter = new CustomFilterOutputStream(fos);
    
                // Записываем данные в фильтрующий поток вывода
                filter.write("Hello, world!".getBytes());
    
                // Закрываем потоки
                filter.close();
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    
    // Пример пользовательского фильтра, расширяющего FilterOutputStream
    class CustomFilterOutputStream extends FilterOutputStream {
        public CustomFilterOutputStream(OutputStream out) {
            super(out);
        }
    
        @Override
        public void write(byte[] b) throws IOException {
            // Добавляем дополнительную функциональность перед записью данных
            System.out.println("Before writing data");
    
            // Вызываем метод write() суперкласса для записи данных в поток вывода
            super.write(b);
    
            // Добавляем дополнительную функциональность после записи данных
            System.out.println("After writing data");
        }
    }
```


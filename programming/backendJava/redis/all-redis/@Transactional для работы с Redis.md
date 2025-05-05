---
title: "@Transactional для работы с Redis"
tags:
  - Redis
related_topics: 
created: 2024-09-23 17:29
modified: 2024-09-23T17:32:58+03:00
questions: 
notes: 
links: 
---


**Spring Data Redis** поддерживает транзакции через Redis и позволяет использовать аннотацию **`@Transactional`**. Однако важно понимать, что эта поддержка работает на уровне **RedisTemplate** и его механизма транзакций (**MULTI/EXEC**), а не как полноценная аналогия транзакций в реляционных базах данных с rollback-ом и ACID-семантикой.

### Как работает поддержка транзакций Redis в Spring:

1. **Redis транзакции через Spring Data Redis**: В Spring Data Redis можно использовать аннотацию **`@Transactional`** для работы с Redis, но это работает с ограничениями модели транзакций Redis, где выполняется последовательность команд, но без полной изоляции и откатов в случае ошибок. Spring просто группирует Redis команды и отправляет их на выполнение через Redis механизм **MULTI/EXEC**.

### Пример использования `@Transactional` в Redis через Spring Data Redis:
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class RedisTransactionService {

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @Transactional
    public void executeRedisTransaction() {
        redisTemplate.opsForValue().set("key1", "value1");
        redisTemplate.opsForValue().increment("counter", 1);
        // Если возникнет исключение, транзакция не откатится, но все команды будут в очереди и отправлены с MULTI/EXEC
    }
}

```

### Что происходит при использовании `@Transactional` с Redis:

1. **Атомарность**: Все команды, включённые в транзакцию, группируются и выполняются как единое целое с помощью команды Redis **`EXEC`**.
    
2. **Изоляция**: Redis транзакции не дают полной изоляции на уровне ACID. Важно понимать, что команды Redis внутри транзакции выполняются последовательно, но другие команды от других клиентов могут воздействовать на ключи между вызовами. Это особенно важно для понимания работы с командой **`WATCH`**, которая проверяет состояние ключей перед транзакцией.
    
3. **Нет откатов (Rollback)**: Redis не поддерживает откат транзакций, если одна из команд завершается с ошибкой. Это значит, что если одна команда в транзакции завершится с ошибкой, другие команды всё равно будут выполнены. Это поведение сохраняется и при использовании **`@Transactional`** в Spring.
    
4. **Команда `WATCH`**: Если вы используете `WATCH`, то Redis транзакция отменяется, если ключ, который отслеживается, был изменён другим клиентом. Это помогает управлять состоянием данных и избегать гонок за ресурсы.
    

### Пример с использованием `@Transactional` и `WATCH`:
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class RedisTransactionService {

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @Transactional
    public void safeIncrement(String key) {
        redisTemplate.watch(key); // Отслеживание изменений ключа перед началом транзакции
        String currentValue = redisTemplate.opsForValue().get(key);

        redisTemplate.multi(); // Начало транзакции (MULTI)
        redisTemplate.opsForValue().increment(key, 1);

        // Завершение транзакции (EXEC)
        List<Object> result = redisTemplate.exec();

        if (result == null) {
            throw new RuntimeException("Transaction failed due to key modification");
        }
    }
}

```

### Важные моменты:

1. **Как `@Transactional` работает в Spring Data Redis**: При использовании аннотации `@Transactional` в контексте Redis, Spring по сути использует механизмы **MULTI/EXEC** для выполнения транзакций. Все команды, которые должны быть выполнены внутри транзакции, отправляются на выполнение только после вызова **`EXEC`**. Это означает, что внутри аннотированного метода `@Transactional` команды Redis фактически не выполняются сразу, а добавляются в очередь и затем выполняются в атомарной последовательности.
    
2. **Поддержка отката (rollback)**: В Spring Data Redis аннотация `@Transactional` **не предоставляет откат транзакции в классическом виде**, как это происходит с реляционными базами данных. Однако, если возникает исключение до вызова `EXEC`, все команды транзакции будут просто сброшены, так как фактическое выполнение не произошло.
    
3. **Ограничение по типам исключений**: Если метод, аннотированный `@Transactional`, выбрасывает непроверенное исключение (например, `RuntimeException`), транзакция будет отклонена и не выполнится. Однако, если команда внутри транзакции вызовет ошибку (например, `INCR` над нечисловым значением), другие команды всё равно будут выполнены, так как Redis не поддерживает откат уже выполненных команд.
    

### Пример поведения с исключениями:
```java
@Service
public class RedisTransactionService {

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @Transactional
    public void executeFaultyTransaction() {
        redisTemplate.opsForValue().set("key1", "value1");
        
        // Ошибка: эта команда вызовет исключение, так как ключ содержит строку, а не число
        redisTemplate.opsForValue().increment("key1", 1); 

        redisTemplate.opsForValue().set("key2", "value2");
    }
}

```

В данном примере первая команда (`SET key1 "value1"`) будет выполнена успешно, но `INCR key1` вызовет ошибку. Однако Redis выполнит **`SET key2 "value2"`**, так как Redis не отменяет транзакцию при ошибке команды.

### Заключение:

Redis действительно поддерживает транзакции с помощью аннотации **`@Transactional`** в Spring Data Redis, но важно понимать, что это не то же самое, что транзакции в реляционных базах данных. Redis транзакции работают через механизмы **MULTI/EXEC** и имеют ограничения:

- Нет поддержки полного отката (rollback) транзакций.
- Ошибки внутри транзакций не прерывают выполнение других команд.
- Использование **`WATCH`** для управления конкурентностью важно для корректной работы в многопользовательских сценариях.

Аннотация `@Transactional` в контексте Redis в основном используется для того, чтобы Spring автоматически запускал транзакцию через RedisTemplate и позволял управлять группой команд как атомарной операцией.



По умолчанию `RedisTemplate`не участвует в управляемых транзакциях Spring. Если вы хотите `RedisTemplate`использовать транзакцию Redis при использовании `@Transactional`или `TransactionTemplate`, вам необходимо явно включить поддержку транзакций для каждого из них, `RedisTemplate`установив `setEnableTransactionSupport(true)`. Включение поддержки транзакций привязывается `RedisConnection`к текущей транзакции, поддерживаемой `ThreadLocal`. Если транзакция завершается без ошибок, транзакция Redis фиксируется с помощью `EXEC`, в противном случае откатывается с помощью `DISCARD`. Транзакции Redis ориентированы на пакетную обработку. Команды, выданные во время текущей транзакции, ставятся в очередь и применяются только при фиксации транзакции.

Spring Data Redis различает команды только для чтения и команды записи в текущей транзакции. Команды только для чтения, такие как `KEYS`, передаются в свежий (не привязанный к потоку) поток, `RedisConnection`чтобы разрешить чтение. Команды записи ставятся в очередь `RedisTemplate`и применяются после фиксации.

В следующем примере показано, как настроить управление транзакциями:

Пример 1. Конфигурация, включающая управление транзакциями

```java
```java
@Configuration
@EnableTransactionManagement                                 
public class RedisTxContextConfiguration {

  @Bean
  public StringRedisTemplate redisTemplate() {
    StringRedisTemplate template = new StringRedisTemplate(redisConnectionFactory());
    // explicitly enable transaction support
    template.setEnableTransactionSupport(true);              
    return template;
  }

  @Bean
  public RedisConnectionFactory redisConnectionFactory() {
    // jedis || Lettuce
  }

  @Bean
  public PlatformTransactionManager transactionManager() throws SQLException {
    return new DataSourceTransactionManager(dataSource());   
  }

  @Bean
  public DataSource dataSource() throws SQLException {
    // ...
  }
}
```

|     |                                                                                                                                                                                                                                                                                        |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | Настраивает контекст Spring для включения [декларативного управления транзакциями](https://docs.spring.io/spring-framework/reference/6.1/data-access.html#transaction-declarative) .                                                                                                   |
|     | Настраивает `RedisTemplate`участие в транзакциях путем привязки соединений к текущему потоку.                                                                                                                                                                                          |
|     | Для управления транзакциями требуется `PlatformTransactionManager`. Spring Data Redis не поставляется с `PlatformTransactionManager`реализацией. Если ваше приложение использует JDBC, Spring Data Redis может участвовать в транзакциях, используя существующие менеджеры транзакций. |


---
title: Введение в Redis
tags:
  - Redis
related_topics: 
created: 2024-09-23 15:18
modified: 2024-09-23T17:15:52+03:00
questions: 
notes: 
links: 
---

------
[[Redis Streams]]
[[Kafka vs Redis]]
[[Транзакции в Redis]] 

------


<mark class="hltr-red">Redis (Remote Dictionary Server) </mark>— это сверхбыстрая система управления <mark class="hltr-green2">базами данных в памяти, работающая с парадигмами типа "ключ-значение" и поддерживающая различные структуры данных (строки, списки, множества, хэши и т.д.).</mark> Redis часто<mark class="hltr-yellow"> используется для кэширования, управления сессиями, очередей сообщений</mark> и многих других задач. Давай подробно разберём все нюансы и особенности Redis, особенно в контексте реальных кейсов и интеграции в **Spring Boot** на уровне Senior разработчика.

## 1. **Основные концепции Redis**

Redis — это нереляционная, in-memory база данных с возможностью сохранения данных на диск (persistence). Его главное преимущество — это невероятно высокая скорость благодаря тому, что все данные хранятся в оперативной памяти. <mark class="hltr-yellow">Однако существуют механизмы для сохранения данных на диск с помощью</mark> **RDB (Redis Database Backup)** и **AOF (Append-Only File)**.

### Особенности:

- **In-memory**: Данные хранятся в памяти, что позволяет получать почти мгновенные ответы на запросы.
- **Асинхронные репликации**: Redis поддерживает репликацию между несколькими узлами (мастер-слейв).
- **Поддержка сложных структур данных**: строки, списки, множества, упорядоченные множества, хэши, битовые поля, геопространственные индексы и т.д.
- **Поддержка Lua-скриптов**: для выполнения транзакций и сложной логики на стороне Redis.
- **Кластеры и шардирование**: Redis позволяет масштабироваться горизонтально.

### Применение Redis:

- Кэширование данных
- Хранение сессий
- Очереди задач
- Pub/Sub (система публикации-подписки для обмена сообщениями)
- Лимитирование запросов (rate limiting)
- Хранение счетчиков и статистики

## 2. **Структуры данных Redis**

Redis поддерживает разные типы структур данных, что делает его очень гибким для различных задач.
### Типы данных:

- **String (строка)**: Это<mark class="hltr-yellow"> базовый тип данных</mark>. Redis обрабатывает<mark class="hltr-yellow"> строковые значения с O(1) временем доступа</mark>. Примеры использования:
    
    - <mark class="hltr-purple">Кэширование строковых значений </mark>(например, JSON-ответов)
    - <mark class="hltr-purple">Счетчики</mark> (инкремент, декремент)
    
    Пример кода на Spring Boot:
```java
@Autowired
private RedisTemplate<String, String> redisTemplate;

public void setValue(String key, String value) {
    redisTemplate.opsForValue().set(key, value);
}

public String getValue(String key) {
    return redisTemplate.opsForValue().get(key);
}

```

- **List (список)**: Позволяет добавлять элементы в начало или конец списка. Это полезно для реализации очередей (FIFO, LIFO).
```java
public void pushToList(String key, String value) {
    redisTemplate.opsForList().leftPush(key, value);
}

public List<String> getList(String key) {
    return redisTemplate.opsForList().range(key, 0, -1);
}

```

- **Set (множество)**: Хранит уникальные элементы без упорядоченности. Полезно для хранения уникальных значений (например, наборы тэгов или идентификаторов).
```java
public void addToSet(String key, String value) {
    redisTemplate.opsForSet().add(key, value);
}

public Set<String> getSet(String key) {
    return redisTemplate.opsForSet().members(key);
}

```

- **Hash (хэш)**: Это ассоциативный массив, который используется для хранения объектов (свойства и их значения). Полезно для хранения сложных структур данных без необходимости сериализации.
```java
public void putToHash(String key, String field, String value) {
    redisTemplate.opsForHash().put(key, field, value);
}

public Object getFromHash(String key, String field) {
    return redisTemplate.opsForHash().get(key, field);
}

```

- **Sorted Set (упорядоченное множество)**: Это множество с приоритетом (весом), где каждый элемент имеет "score" (оценку). Полезно для создания рейтингов или систем с приоритетами.
```java
public void addToSortedSet(String key, String value, double score) {
    redisTemplate.opsForZSet().add(key, value, score);
}

public Set<String> getSortedSet(String key) {
    return redisTemplate.opsForZSet().range(key, 0, -1);
}

```


## 3. **Тонкости и нюансы использования Redis**

### 3.1. **Кэширование**

Redis часто используется для кэширования данных, что позволяет снизить нагрузку на основную базу данных и ускорить доступ к часто запрашиваемым данным.

#### Пример кэширования с помощью Spring Cache:
```java
@Cacheable(value = "products", key = "#id")
public Product getProductById(Long id) {
    // Логика получения продукта из БД
}

```

Конфигурация RedisCacheManager:
```java

@Configuration
@EnableCaching
public class RedisConfig {

    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory redisConnectionFactory) {
        RedisCacheConfiguration cacheConfig = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10))
            .disableCachingNullValues();

        return RedisCacheManager.builder(redisConnectionFactory)
            .cacheDefaults(cacheConfig)
            .transactionAware()
            .build();
    }
}

```

### 3.2. **Pub/Sub (публикация и подписка)**

Redis поддерживает модель Pub/Sub, что позволяет строить системы для обмена сообщениями между разными сервисами.
```java
@Service
public class RedisPublisher {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    public void publish(String topic, String message) {
        redisTemplate.convertAndSend(topic, message);
    }
}

```

Слушатель: 
```java
@Service
public class RedisSubscriber {

    @RedisMessageListenerContainer
    public void onMessage(Message message, byte[] pattern) {
        System.out.println("Received message: " + new String(message.getBody()));
    }
}

```

### 3.3. **Distributed Lock (распределённые блокировки)**

Redis можно использовать <mark class="hltr-yellow">для создания распределённых блокировок, которые часто необходимы в микросервисах для координации доступа к общим ресурсам.</mark>

```java
public boolean acquireLock(String key, String value, long expireTime) {
    Boolean success = redisTemplate.opsForValue().setIfAbsent(key, value, expireTime, TimeUnit.SECONDS);
    return Boolean.TRUE.equals(success);
}

public void releaseLock(String key, String value) {
    String currentValue = redisTemplate.opsForValue().get(key);
    if (value.equals(currentValue)) {
        redisTemplate.delete(key);
    }
}

```

### 3.4. **Rate Limiting (ограничение запросов)**

Redis используется<mark class="hltr-yellow"> для лимитирования количества запросов от клиента</mark>. Например, <mark class="hltr-purple">можно ограничить пользователя до 10 запросов в секунду</mark>.

```java
public boolean isAllowed(String userId, int limit, int windowInSeconds) {
    String key = "rate_limit:" + userId;
    long currentTime = System.currentTimeMillis() / 1000;

    redisTemplate.multi();
    redisTemplate.opsForZSet().add(key, String.valueOf(currentTime), currentTime);
    redisTemplate.opsForZSet().removeRangeByScore(key, 0, currentTime - windowInSeconds);
    Long count = redisTemplate.opsForZSet().size(key);
    redisTemplate.exec();

    return count != null && count <= limit;
}

```

## 4. **Проблемы и рекомендации**

### 4.1. **Eviction Policy (Политики удаления)**

Redis<mark class="hltr-yellow"> поддерживает несколько стратегий удаления данных, когда память заканчивается:</mark>

- **noeviction**: <mark class="hltr-blue">Не удалять ключи, если память исчерпана.</mark>
- **allkeys-lru**: <mark class="hltr-purple">Удалять наименее недавно использованные ключи.</mark>
- **volatile-lru**: <mark class="hltr-blue">Удалять наименее недавно использованные ключи, у которых установлен срок жизни.</mark>
- **allkeys-random**: <mark class="hltr-purple">Удалять случайные ключи.</mark>

Важно выбирать стратегию в зависимости от типа приложения и требуемого поведения.

### 4.2. **Persistence (сохранение на диск)**

Для долгосрочного хранения данных Redis поддерживает два механизма:

- **RDB (Redis Database Backup)**: <mark class="hltr-yellow">сохраняет состояние базы на определённый момент времени</mark> (снэпшоты).
- **AOF (Append-Only File)**: <mark class="hltr-blue">сохраняет каждую операцию записи.</mark>

Комбинация этих механизмов позволяет добиться как высокой производительности, так и устойчивости к сбоям.

### 4.3. **Master-Slave и Redis Sentinel**

Для обеспечения отказоустойчивости Redis можно настроить в режиме master-slave с автоматическим фейловером при помощи Redis Sentinel, который следит за состоянием мастер-узла.

### 4.4. **Кластеризация и шардирование**

Redis поддерживает кластеризацию, что позволяет масштабировать базу данных горизонтально. Данные разбиваются на шардированные слоты и распределяются по нескольким узлам Redis.

## 5. **Реальные кейсы использования Redis**

1. **Управление сессиями**: Хранение сессий пользователей в Redis позволяет легко масштабировать приложение, так как сессии становятся доступными для всех серверов в кластере.
2. **Очереди задач**: Redis можно использовать как брокер сообщений или простую очередь задач с высокой производительностью.
3. **Система рекомендаций**: Redis удобно использовать для реализации рекомендаций товаров на основе популярности или рейтингов (используя сортированные множества).
4. **Счётчики и метрики**: С Redis легко реализовать системы сбора статистики и счётчиков в реальном времени, особенно благодаря его способности работать с атомарными инкрементами.

Redis — это мощный и гибкий инструмент для множества задач, который идеально интегрируется с Spring Boot, позволяя строить отказоустойчивые, масштабируемые и высокопроизводительные приложения.
---
title: Repository Redis
tags:
  - Redis
related_topics: 
created: 2024-09-23 17:42
modified: 2024-09-23T17:54:26+03:00
questions: 
notes: 
links: 
---

---
[[Отличия RedisTemplate vs Redis Repository]]

---

**Redis Repository** — это механизм, предоставляемый **Spring Data Redis**, который позволяет работать с Redis как с хранилищем данных на высоком уровне абстракции. Он автоматически управляет сохранением, обновлением, удалением и извлечением объектов в Redis, используя шаблон **репозитория** (Repository Pattern). Это аналогично тому, как работают репозитории в Spring Data JPA, MongoDB и других модулях Spring Data.

### Основные возможности Redis Repository:

1. **CRUD-операции**: Spring Data Redis автоматически предоставляет методы для стандартных операций по созданию, чтению, обновлению и удалению данных.
    
2. **Автоматическое отображение объектов**: Spring Data Redis может автоматически преобразовывать объекты Java в сериализованные формы, подходящие для хранения в Redis, и обратно.
    
3. **Поддержка различных типов данных**: Spring Data Redis поддерживает работу с хешами (Hash), строками (String), списками (List), множествами (Set) и другими структурами данных Redis.
    
4. **Кастомизация запросов**: Можно определять свои собственные методы для выполнения запросов и операций с данными в Redis.
    
5. **Кеширование**: Redis Repository хорошо интегрируется с механизмами кеширования в Spring (например, с аннотацией **`@Cacheable`**).
    

### Как это работает:

Redis Repository использует **`RedisTemplate`** или **`ReactiveRedisTemplate`** под капотом для выполнения операций с Redis. Оно автоматически связывает объекты домена с данными в Redis и преобразует их при необходимости.

### Пример использования Redis Repository в Spring Boot

1. **Настройка Maven зависимости**:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
</dependency>

```

2. **Конфигурация подключения к Redis**:
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        
        // Настройка сериализации ключей и значений
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        
        return template;
    }
}

```



3. **Определение сущности (Entity)**:
Для того чтобы сохранять объекты в Redis через репозиторий, нужно создать сущность, которую вы будете хранить. Для этого используется аннотация **`@RedisHash`**.
```java
import org.springframework.data.annotation.Id;
import org.springframework.data.redis.core.RedisHash;

import java.io.Serializable;

@RedisHash("Person")
public class Person implements Serializable {

    @Id
    private String id;
    private String name;
    private int age;

    // Конструкторы, геттеры, сеттеры

```
**`@RedisHash`**: Эта аннотация указывает, что класс будет сохраняться в Redis. Опционально можно указать TTL (время жизни) объекта с помощью параметра **`timeToLive`**.

4. **Создание репозитория**:
Теперь нужно создать интерфейс репозитория, который будет управлять объектами типа **Person**. Для этого можно использовать стандартный интерфейс **`CrudRepository`**, который предоставляет базовые методы работы с данными.
```java
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface PersonRepository extends CrudRepository<Person, String> {
    // Можно добавлять кастомные методы, если нужно
    Person findByName(String name);
}

```
**`CrudRepository`**: Это интерфейс, который предоставляет стандартные методы CRUD для работы с Redis.

5. **Использование репозитория**:
Теперь можно использовать созданный репозиторий для выполнения операций с объектами.
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class PersonService {

    @Autowired
    private PersonRepository personRepository;

    public void savePerson(Person person) {
        personRepository.save(person);
    }

    public Person getPersonById(String id) {
        return personRepository.findById(id).orElse(null);
    }

    public Person getPersonByName(String name) {
        return personRepository.findByName(name);
    }

    public void deletePerson(String id) {
        personRepository.deleteById(id);
    }
}

```

### Расширенные возможности Redis Repository:

1. **Сериализация данных**: Spring Data Redis позволяет настраивать, каким образом объекты будут сериализованы перед сохранением в Redis. Например, можно использовать **Jackson** для сериализации объектов в JSON (как в примере выше).
    
2. **TTL (Time-to-Live)**: Можно настроить время жизни объектов в Redis на уровне класса с помощью аннотации **`@RedisHash`**:
```java
@RedisHash(value = "Person", timeToLive = 60) // Объект будет храниться 60 секунд
public class Person {
    // ...
}

```
3. **Кастомные запросы**: Можно добавлять собственные методы в репозиторий для выполнения специфических операций. Например, можно создавать методы для поиска объектов по имени или возрасту.

### Реактивное использование Redis Repository:

С помощью **Spring Data Redis Reactive** можно создавать реактивные репозитории для асинхронной работы с Redis.
```java
import org.springframework.data.repository.reactive.ReactiveCrudRepository;
import reactor.core.publisher.Mono;

public interface ReactivePersonRepository extends ReactiveCrudRepository<Person, String> {
    Mono<Person> findByName(String name);
}

```

Это позволяет интегрировать Redis с реактивными фреймворками, такими как WebFlux, и выполнять асинхронные операции над данными.

### Заключение:

**Redis Repository** в Spring Data Redis предоставляет удобный и высокоуровневый способ работы с Redis как с хранилищем данных. Оно абстрагирует сложную работу с низкоуровневыми командами Redis и автоматически сериализует объекты Java, управляя их сохранением и извлечением. Это похоже на работу с другими базами данных через Spring Data, такими как JPA или MongoDB.
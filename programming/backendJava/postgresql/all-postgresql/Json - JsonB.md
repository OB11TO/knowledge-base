---
title: Json - JsonB
tags:
  - PostgreSql
related_topics: 
created: 2024-12-04 16:20
modified: 2024-12-09T13:11:06+03:00
questions: 
notes: 
links: 
---


### Полное руководство по типам данных JSON и JSONB в PostgreSQL

#### **1. Что такое JSON и JSONB?**

- **JSON (JavaScript Object Notation)**:
    
    - Это текстовый формат для хранения структурированных данных в виде пар "ключ-значение".
    - Хранит данные как строку, точно в том виде, как они были введены.
- **JSONB (Binary JSON)**:
    
    - Это двоичная версия JSON.
    - Данные парсятся, упорядочиваются и сохраняются в бинарном формате для оптимизации запросов и использования памяти.

### **1. Реальное хранение и отображение**

- **На диске:**  
    JSONB хранится в двоичном формате. Это оптимизированная структура, где данные парсятся, упорядочиваются, дубликаты удаляются, а ключи и значения хранятся в сжатом виде. Это позволяет PostgreSQL эффективно обрабатывать запросы к JSONB, индексировать ключи и быстро искать значения.
    
- **При отображении:**  
    Когда вы делаете запрос к колонке JSONB, PostgreSQL **конвертирует данные обратно в текстовый формат JSON** перед тем, как вернуть их пользователю. Это делается для удобства, чтобы вы могли видеть данные в читаемом виде.

---

#### **2. Для чего используются JSON и JSONB?**

- **Гибкость хранения**:
    
    - Эти типы полезны, когда схема данных может быть динамичной или меняться со временем.
    - Пример: Логирование, хранение пользовательских настроек, интеграция с внешними API, которые возвращают JSON.
- **Моделирование сложных данных**:
    
    - Если данные сложно разложить по обычным таблицам (например, иерархии, вложенные структуры).
- **Хранение временных данных**:
    
    - Когда структура данных неизвестна заранее или может изменяться, а строгая схема не требуется.

---

#### **3. Что хранится внутри JSON и JSONB?**

- **JSON**:
    
    - Текстовая строка с данными в формате JSON.
    - Пример:
        
        
        `{"name": "Alice", "age": 30, "skills": ["Java", "SQL"]}`
        
- **JSONB**:
    
    - Бинарное представление JSON.
    - Хранит те же данные, но упорядочивает ключи, удаляет лишние пробелы и парсит значения.

---

#### **4. Когда применять JSON и JSONB?**

- **JSON**:
    
    - Когда важен оригинальный текст данных.
    - Если нужно сохранить форматирование или пробелы.
    - Для редкого анализа данных, где производительность не важна.
- **JSONB**:
    
    - Когда нужен быстрый доступ к данным.
    - Для поиска, фильтрации или анализа.
    - Когда данные обновляются или сравниваются.

---

#### **5. Отличия между JSON и JSONB**

|Характеристика|JSON|JSONB|
|---|---|---|
|**Формат хранения**|Текстовый|Бинарный|
|**Скорость чтения**|Медленнее|Быстрее|
|**Скорость записи**|Быстрее|Медленнее (из-за парсинга)|
|**Размер на диске**|Больше (хранит пробелы)|Меньше (упорядоченные данные)|
|**Поддержка индексов**|Ограниченная|Полная|
|**Операции с данными**|Ограниченные|Расширенные (больше операторов и функций)|

---

#### **6. Когда какой лучше использовать?**

- Используйте **JSON**:
    
    - Когда данные редко анализируются.
    - Когда важно сохранить исходное форматирование.
    - Если вы работаете с небольшими объемами данных.
- Используйте **JSONB**:
    
    - Для любых случаев, где важна производительность.
    - Для сложных операций поиска или анализа.
    - Если нужно создать индексы.

---

#### **7. Влияет ли это на нормализацию базы данных?**

- **Да, использование JSON/JSONB влияет на нормализацию.**

1. **Когда это полезно?**
    
    - JSON/JSONB удобны для хранения неструктурированных или полуструктурированных данных.
    - Это может заменить дополнительную таблицу, если данные редко запрашиваются.
    
    Пример: Вместо создания таблицы для хранения настроек пользователя, можно хранить их в JSONB-колонке:
    
    sql
    
    Копировать код
    
    `user_id | settings --------|-------------------------------- 1       | {"theme": "dark", "lang": "en"}`
    
2. **Чем это опасно?**
    
    - Использование JSON/JSONB может нарушить строгую нормализацию:
        - Дублирование данных.
        - Усложнение анализа.
        - Трудности с обновлением или поддержкой.
    - Злоупотребление JSONB может привести к отсутствию четкой схемы данных, что противоречит принципам реляционной модели.
3. **Рекомендации**:
    
    - Использовать JSON/JSONB как дополнение, а не замену строгой реляционной модели.
    - Не хранить ключевые бизнес-данные в JSONB, если можно представить их в виде таблиц.

---

#### **8. Как JSON и JSONB реализованы под капотом?**

- **JSON**:
    
    - Хранится как строка в текстовом формате.
    - Парсинг выполняется каждый раз при запросе.
- **JSONB**:
    
    - При записи:
        - Данные парсятся.
        - Ключи упорядочиваются.
        - Удаляются пробелы и дубликаты ключей.
        - Сохраняется бинарное дерево.
    - При запросах:
        - Запросы обрабатываются быстрее за счет бинарной структуры.

---

#### **9. Какие индексы используются?**

- Для **JSON** индексы практически не поддерживаются, так как данные хранятся как строка.
    
- Для **JSONB**:
    
    - **GIN (Generalized Inverted Index)**:
        
        - Самый популярный индекс.
        - Ускоряет поиск по ключам и значениям.
        
        
        `CREATE INDEX idx_jsonb ON table_name USING GIN(jsonb_column);`
        
    - **B-tree**:
        
        - Для индексирования конкретных путей JSONB, если нужно точное совпадение.
        
        
        `CREATE INDEX idx_path ON table_name ((jsonb_column->>'key'));`
        
    - **GiST**:
        
        - Используется реже, но подходит для более сложных запросов.
        
        
        `CREATE INDEX idx_gist ON table_name USING GiST(jsonb_column);`
        

---

#### **10. Best Practices**

1. **Используйте JSONB для анализа**:
    
    - JSONB быстрее и компактнее.
    - Создавайте индексы на часто используемые ключи.
2. **Не злоупотребляйте JSON/JSONB**:
    
    - Не используйте их как универсальный тип. Они не заменяют таблицы.
    - Если структура данных известна заранее, лучше нормализовать данные.
3. **Оптимизируйте запросы**:
    
    - Избегайте поиска без индексов.
    - Если работаете с большими JSONB-документами, извлекайте только нужные данные:
        
        `SELECT jsonb_column->>'key' FROM table_name;`
        
4. **Храните только нужные данные**:
    
    - Не храните большие объемы неиспользуемой информации в JSONB, чтобы избежать роста размера базы данных.

---

#### **11. Когда не использовать JSON/JSONB?**

- **Когда данные могут быть представлены в виде таблиц**:
    
    - Если структура данных известна и стабильна, лучше нормализовать.
- **Когда важна строгая схема**:
    
    - JSON/JSONB позволяет хранить любые данные, но это увеличивает вероятность ошибок.
- **Когда важно быстрое обновление**:
    
    - JSONB обновляется медленно, особенно при большом объеме данных.

---

#### **12. Пример: Сравнение JSON и реляционного подхода**

- JSONB:
    
    sql
    
    Копировать код
    
    `CREATE TABLE users (     id SERIAL PRIMARY KEY,     profile JSONB ); INSERT INTO users (profile) VALUES ('{"name": "Alice", "age": 30}');`
    
- Нормализованная структура:
    
    sql
    
    Копировать код
    
    `CREATE TABLE users (     id SERIAL PRIMARY KEY,     name TEXT,     age INT ); INSERT INTO users (name, age) VALUES ('Alice', 30);`
    

---

### Итог

- **JSON** — для сохранения оригинального текста.
- **JSONB** — для анализа и запросов.
- Используйте с осторожностью, чтобы не превратить реляционную базу в хранилище без структуры.
- Оптимизируйте запросы и храните данные только в случае необходимости.
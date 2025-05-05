---
title: FilterWriter
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:24
modified: 2024-09-11T11:24:19+03:00
questions: 
notes: 
links: 
---
### 7. **`FilterWriter`**

- **Описание**: Базовый класс для потоков, которые добавляют дополнительную функциональность к другим потокам вывода (<mark class="hltr-red">декораторы</mark>).
- **Особенности**:
    - Не используется напрямую, но служит основой для других классов-декораторов, таких как `BufferedWriter`.
- **Пример**:
    - Классы, такие как `BufferedWriter`, наследуются от `FilterWriter`.

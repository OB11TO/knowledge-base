---
title: FilterReader
tags:
  - IO-NIO
related_topics: 
created: 2024-09-11 11:20
modified: 2024-09-11T11:20:40+03:00
questions: 
notes: 
links: 
---
### 7. **`FilterReader`**

- **Описание**: Базовый класс для потоков, которые добавляют дополнительную функциональность к другим потокам ввода (<mark class="hltr-red">декораторы</mark>).
- **Особенности**:
    - Сам по себе редко используется, но служит основой для других классов-декораторов, таких как `BufferedReader`.
- **Пример**:
    - Классы, такие как `BufferedReader`, наследуются от `FilterReader`.
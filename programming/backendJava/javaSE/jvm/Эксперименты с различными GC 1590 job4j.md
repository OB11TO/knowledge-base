---
title: Эксперименты с различными GC 1590 job4j
tags:
  - JVM
  - Job4j
related_topics: 
created: 2025-02-04 15:05
modified: 2025-02-04T15:20:59+03:00
questions: 
notes: 
links: 
---

![[Pasted image 20250204150559.png]]

### 🔹 **Как вычисляются размеры Young и Old Generation?**

Обычно по умолчанию:

- **Young Generation** — занимает **⅓ от всего хипа (heap)**
- **Old Generation** — занимает **⅔ от всего хипа**

Если у нас **хип = 12 МБ**, то:

- **Old Generation ≈ 8 МБ** _(2/3 от 12 МБ)_
- **Young Generation ≈ 4 МБ** _(1/3 от 12 МБ)_

---

### 🔹 **Как распределяется Young Generation?**

Молодое поколение (`Young Gen`) состоит из:

- **Eden Space** (основное пространство для новых объектов)
- **Survivor 0 (S0)**
- **Survivor 1 (S1)**

💡 **Обычно соотношение** между `Eden` и `Survivor` **по умолчанию = 8:1:1**  
(т.е. `Eden` занимает **80%**, `S0` и `S1` по **10%** от Young Generation).

<mark class="hltr-red">Или говорят, что 1/6 от eden</mark>

📌 Для нашего случая:

- **Young Generation = 4 МБ**
- **Eden ≈ 80%** → **3.3 МБ**
- **Survivor 0 (S0) ≈ 10%** → **0.33 МБ**
- **Survivor 1 (S1) ≈ 10%** → **0.33 МБ**

👉 Именно так получились указанные тобой цифры!

---

### 🔹 **Формулы для расчёта размеров хипа**

Общий хип (`Heap`) = **Old Generation** + **Young Generation**

Если следовать стандартному разбиению 2:1:

- **Old Gen = (2/3) * Heap**
- **Young Gen = (1/3) * Heap**
    - **Eden = 80% от Young Gen**
    - **Survivor 0 = 10% от Young Gen**
    - **Survivor 1 = 10% от Young Gen**

Если заданы флаги JVM `-Xms12M -Xmx12M` (фиксированный хип 12 МБ),  
то расчёты такие же.

---
### 🔹 **Как изменить размеры в JVM?**

Можно управлять размером хипа через параметры JVM:
```java
java -Xms12M -Xmx12M -XX:NewRatio=2 -XX:SurvivorRatio=6
```

- `-XX:NewRatio=2` → делит хип в **соотношении 2:1** между **Old** и **Young**
- `-XX:SurvivorRatio=6` → делит Young Gen в соотношении **Eden:Survivor0:Survivor1 = 6:1:1**

Если задать `-XX:NewRatio=1`, то `Old:Young` будет **1:1** (50% на 50%).

----

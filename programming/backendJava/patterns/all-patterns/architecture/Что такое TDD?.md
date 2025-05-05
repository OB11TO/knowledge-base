---
title: Что такое TDD?
tags:
  - Pattern
  - Architecture
related_topics: 
created: 2025-02-13 16:21
modified: 2025-02-13T16:34:44+03:00
questions: 
notes: 
links: 
---


Полезный код - это код, который сразу выполняет требования клиента. 

Недостаток такого подхода в том, что мы можем ошибиться в дизайне. Если такое произойдет, то нам нужно переписать уже проверенный код. Делать редизайн.

Другой подход создания программы - э<mark class="hltr-green2">то начать описывать поведение системы через тесты.</mark> <mark class="hltr-yellow">Требования клиента формируется на основе тест-кейсов. Тест-кейс это описание того, что должен получить клиент или заказчик в какой-то конкретной ситуации при соблюдении определенных условий. Тест-кейс находит свое воплощение в Unit тестах. При этом как система будет достигать этих требований пока неизвестно. Это будет определено в ходе реализации.</mark>

<mark class="hltr-red">Любую систему можно описать через интерфейсы взаимодействия. </mark>
Такой поход называется программирование через тесты - <mark class="hltr-pink">Test Driven Development.</mark>

![[Pasted image 20250213162425.png]]
```java
package ru.job4j.ood.tdd;

import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;

import java.util.Calendar;
import java.util.List;

@Disabled("Тесты отключены. Удалить аннотацию после реализации всех методов по заданию.")
public class Cinema3DTest {
    @Test
    public void whenBuyThenGetTicket() {
        Account account = new AccountCinema();
        Cinema cinema = new Cinema3D();
        Calendar date = Calendar.getInstance();
        Ticket ticket = cinema.buy(account, 1, 1, date);
        assertThat(ticket).isEqualTo(new Ticket3D());
    }

    @Test
    public void whenAddSessionThenItExistsBetweenAllSessions() {
        Cinema cinema = new Cinema3D();
        Session session = new Session3D();
        cinema.add(session);
        List<Session> sessions = cinema.find(data -> true);
        assertThat(sessions).contains(session);
    }

    @Test
    public void whenBuyOnInvalidRowThenGetException() {
        Account account = new AccountCinema();
        Cinema cinema = new Cinema3D();
        Calendar date = Calendar.getInstance();
        assertThatThrownBy(() -> cinema.buy(account, -1, 1, date))
                .isInstanceOf(IllegalArgumentException.class);
    }
}
```


------

## Test Driven Development (TDD) vs Test Last Development (TLD) ⚖️

### Test Driven Development (TDD) 🚀

- **Суть:**  
    При TDD тесты пишутся **до** реализации функционала. Разработчик сначала формулирует требование в виде теста, который на начальном этапе не проходит (первый шаг – «Red»). Затем он пишет минимально необходимый код, чтобы тест прошёл («Green»), и, наконец, проводит рефакторинг («Refactor»).
    
- **Преимущества:**
    
    - **Раннее обнаружение ошибок:** ошибки выявляются ещё до написания основного кода.
    - **Дизайн через тесты:** требования к функционалу формулируются через тесты, что помогает создавать более модульный и тестируемый код.
    - **Улучшенная архитектура:** постоянная проверка кода тестами способствует поддержке и рефакторингу.
- **Пример рабочего цикла:**
    
    1. **Написание теста:** Пишется тест, который проверяет, что функция сложения возвращает правильный результат.
    2. **Реализация кода:** Пишется минимальный код, чтобы тест прошёл.
    3. **Рефакторинг:** Код упрощается и улучшается, при этом тесты продолжают проходить.

---

### Test Last Development (TLD) 🕒

- **Суть:**  
    При TLD разработчик сначала пишет **код**, а тесты создаются уже после реализации функционала. Этот подход является более традиционным и часто используется в ситуациях, когда тестирование является дополнительной проверкой уже готового продукта.
    
- **Преимущества и недостатки:**
    
    - **Плюсы:**
        - Возможно быстрее можно написать первоначальную версию функционала, когда тесты не мешают процессу разработки.
    - **Минусы:**
        - Ошибки могут обнаруживаться позже, что увеличивает стоимость их исправления.
        - Код может оказаться менее тестируемым, что усложняет последующий рефакторинг и поддержку.
        - Архитектура может не быть оптимизированной для тестирования, так как тесты добавляются «на последок».
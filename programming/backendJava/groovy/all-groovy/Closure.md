---
title: Closure
tags:
  - Groovy
related_topics: 
created: 2024-09-17 18:15
modified: 2024-09-18T13:38:33+03:00
questions: 
notes: 
links: 
---

---
[[Closure info]]

----


### Closure

<mark class="hltr-red">Замыкания (Closure)</mark> - это один из самых мощных инструментов в программировании. Он <mark class="hltr-yellow">существует и в Java в виде lambda выражений, в которых также можно использовать переменные вне области lambda выражений</mark>. Но<mark class="hltr-green2"> в Groovy пошли еще дальше</mark>

- По сути ==похож== на функциональный интерфейс. Мы также можем передавать его в Stream, как и функциональный интерйфес.

![[images/Untitled 18 2.png|Untitled 18 2.png]]

##### Closure абстрактный класс
![[images/Untitled 19 2.png|Untitled 19 2.png]]
Похож на функциональный интерфейс, но<mark class="hltr-red"> содержит несколько методов</mark> `call(...)`

![[images/Untitled 21 2.png|Untitled 21 2.png]]
![[Pasted image 20240918121500.png]]


- Для простоты ==можно не вызывать== `call()` явно, он сам вызовется.
![[Pasted image 20240918121621.png]]
- Также чтобы отличать closure от функциональных интерфейсов <mark class="hltr-yellow">для { } не нужно указывать return</mark>, а<mark class="hltr-green2"> в лямбда нужно</mark>
- Есть ключевое зарезервированное слово `it`, место `value`

![[images/Untitled 20 2.png|Untitled 20 2.png]] ![[Pasted image 20240918121845.png]]

##### Пример с методом
![[Pasted image 20240918122053.png]]
![[Pasted image 20240918123419.png]]
#### Замыкание
- ==Если== ==функция====, такая, как <mark class="hltr-red">check</mark> ==принимает функцию или возвращает её,== ==то она называется функцией== ==высшего порядка==

![[images/Untitled 22 2.png|Untitled 22 2.png]]

<mark class="hltr-red">ВОТ ЭТО ВЫЗОВ МЕТОДА  ПО СУТИ </mark>

![[Pasted image 20240918122918.png]]

- На самом деле ==под капотом создаются локальные классы==

![[images/Untitled 24 2.png|Untitled 24 2.png]]

  

**==Есть еще очень важные поля==**

- `closure.thisObject` - указывает на this объект, там где мы его определили
- `closure.owner` - там где определили closure и к этого объекту обращаемся.
- `closure.delegate` - по умолчанию owner, можно поменять вручную для DSL

![[images/Untitled 25 2.png|Untitled 25 2.png]]

![[images/Untitled 26 2.png|Untitled 26 2.png]]

- `closure.delegate` - переопределяем для делегирования

![[images/Untitled 27 2.png|Untitled 27 2.png]]
- Теперь <mark class="hltr-green2">closure делегирует запрос</mark> на Student

![[images/Untitled 28 2.png|Untitled 28 2.png]]

- `Паттерн делегирования` - ==Все запросы которые приходя==т `Closure`, мы ==делегируем== ==объекту==.
- ==Аналог==, лучше писать `with`

![[images/Untitled 29 2.png|Untitled 29 2.png]]

  

==ДЛЯ ОЗНАКОМЛЕНИЯ.==

![[images/Untitled 30 2.png|Untitled 30 2.png]]


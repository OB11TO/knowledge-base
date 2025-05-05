---
title: Dmdev Date and Time
tags:
  - JavaSE
related_topics: 
created: 2024-09-04 15:51
modified: 2025-01-23T14:31:39+03:00
difficulty: easy
questions: 
notes: 
links:
  - https://www.youtube.com/watch?v=5QzLsYQRt0I
---
 

![[Pasted image 20240904160139.png]]

- <mark class="hltr-red">1 января 1970 года точка отсчета (instant)</mark>
- <mark class="hltr-orange">Instant</mark> - это <mark class="hltr-green2">по сути точка на timeline. Момент времени на timeline. </mark> 

![[Pasted image 20240904160225.png]]



![[Pasted image 20240904160344.png]]




![[Pasted image 20240904160427.png]]


 - <mark class="hltr-yellow">Физическое время мы говорим о всех событиях, которые случаются на нашей планете Земля, оно одинаково для всего живово.</mark>
 - Мы можем говорить о том, что происходит сейчас, в прошлом, в будущем.
 - <mark class="hltr-red">НО не можем говорить, что сегодня среда или увидимся в воскресенья и т.д. Потому что они не относятся к физическому времени. </mark>
 

![[Pasted image 20240904160614.png]]

- Гражданское время !!!! Мы привыкли говорить в нем о времени. 
- <mark class="hltr-purple">Используем календари, месяцы, дни и так далее</mark>
![[Pasted image 20240904160734.png]]



### Теперь есть периоды и DateTime, чтобы было более нагляднее для людей
![[Pasted image 20240904160916.png]]
- <mark class="hltr-red">Теперь Periods измеряется не в секундах, как Duration</mark> 

![[Pasted image 20240904161040.png]]

- <mark class="hltr-green2">Поэтому можно сделать вывод, что все математические операции и баги будут происходить на вычислении Periods and Datetime</mark>


![[Pasted image 20240904161257.png]]

 - <mark class="hltr-purple">Значит нужно преобразовывать Civil Time в Phisycal Time делать математические расчеты и преобразовывать обратно !</mark>
- <mark class="hltr-orange">Time Zone помогает правильно преобразовывать </mark>

![[Pasted image 20250123141342.png]]


![[Pasted image 20250123141410.png]]



![[Pasted image 20250123141449.png]]
 

![[Pasted image 20240904161647.png]]



#### JAVA CODE 

- <mark class="hltr-red">Temporal</mark> класс -  э<mark class="hltr-yellow">то базовый интерфейс, от которого реализуют множество других классов</mark>. <mark class="hltr-blue">Например Instant</mark>


![[Pasted image 20240904162120.png]]

----


![[Pasted image 20240904162139.png]]

![[Pasted image 20250123142014.png]]
![[Pasted image 20250123142042.png]]

----

#### Практика
 ![[Pasted image 20250123142843.png]]
 - <mark class="hltr-red">Ошибка разработчика data api, что можно передавать MONTHS. Нельзя передавать, будет ошибка, так как это физическое время.</mark>

- <mark class="hltr-green2">Нужно использовать Civil Time для месяцев.</mark> <mark class="hltr-orange">НО НУЖНО БЫТЬ АККУРАТНЕЕ, ТАК КАК ОПЕРАЦИИ ПЛОХО ОПРЕДЕЛЕНЫ И МОЖНО ПОЛУЧИТЬ БАГ</mark>
![[Pasted image 20250123143019.png]]



---
title: DNS
tags:
  - Network
related_topics: 
created: 2024-09-12 18:17
modified: 2024-11-06T17:25:17+03:00
questions: 
notes: 
links: 
---

### DNS

Доменное имя представляет из себя понятный и хорошо запоминающийся человеку текст, например google.com

С каждым доменным именем связывается один или несколько IP адресов, и в свою очередь, с каждым IP адресом может быть связано одно или несколько доменных имен.

<mark class="hltr-yellow">Cуществуют два способа реализации системы доменных имен:</mark>

1. был основан на использовании <mark class="hltr-purple">файла hosts</mark>, который представляет из себя обычный текстовый файл, где хранятся пары соответствий IP адресов и доменных имен.
    
    Windows - `C:\windows\system32\drivers\etc\hosts`
    
    Unix - `/etc/hosts`
    
2. <mark class="hltr-green2">Второй способ реализации доменных имен основан на использовании службы DNS</mark>  
    DNS (Domain Name System) - это распределенная система, в которой информация о доменах хранится на большом количестве связанных между собой DNS-серверов.  
    

<mark class="hltr-yellow">Каждый DNS-сервер хранит информацию о доменах только своей зоны</mark>

Но т.к. все DNS серверы связаны между собой, то можно получить необходимую информацию( т.е. IP адрес по доменному имени) вне зависимости от того, к какому серверу вы обратились.

DNS предполагает, что все компьютеры в сети разделяются на логические группы - домены.

При этом доменные имена образуют иерархическую структуру, т.к одни домены могут являться частью других

В этой связи выделяют домены первого уровня, второго и т.д.

![[Untitled 23.png]]

Переход с домена на домен осуществляется справа-налево

![[Untitled 24.png]]

Обычно DNS используется для преобразования доменных имен в IP адреса - это называется прямое преобразование.

Обратное преобразование - по известному IP адресу получить доменное имя.

Для обратного преобразования в DNS используются обратные зоны, которые создаются и настраиваются независимо от прямых.

Созданием и поддержанием имен в доменах верхнего уровня занимаются спцеиальные компании - регистраторы доменных имен.

Сами домены верхнего уровня создаются и поддерживаются на уровне корневого домена. Этим занимается международная организация.



![[Untitled 120.png]]

![[Untitled 121.png]]

![[Untitled 122.png]]

![[Untitled 123.png]]

![[Untitled 124.png]]

![[Untitled 125.png]]

- ==Происходит цепочка вызовов нужных доменов по уровням.==
- После ==получения доменого имени и IP адреса происходит уже запрос по IP==
- ==Кешируются== такие запросы локально и в днс серере (сразу вернут адрес ip)

==**Как это выглядит**==

![[Untitled 126.png]]

==Сначала в приоритете проверяется локальный файл== ==`hosts`====, если там не находится информация, которая нужна,то запрос идет по рутовоу доменному имени, получает== ==`ip`== ==следующей зоны и тд.==

---
title: Aнонимные классы
tags:
  - FunctionsP
related_topics: 
created: 2024-09-10 14:04
modified: 2025-03-24T15:59:50+03:00
questions: 
notes: 
links: 
---

## Краткое описание
- В Java <mark class="hltr-green2">анонимные классы имеют доступ только</mark> к `final` или <mark class="hltr-yellow">effectively final</mark> переменным из внешнего контекст
- Aнонимный класс<mark class="hltr-yellow"> не может содержать статические переменные и методы</mark>
- <mark class="hltr-purple">Можно через рефлексию создать произвольное количество объектов анонимного класса</mark>
![[images/Untitled 5.png|Untitled 5.png]]
## Полное описание
Анонимный класс — это полноценный внутренний класс. ==(Реализация от класса или от интерфейса)== Поэтому у него ==есть доступ к переменным внешнего класса, в том числе к статическим и приватным:==

```Java
public class Main {

   private static int currentErrorsCount = 23;

   public static void main(String[] args) {

       MonitoringSystem errorModule = new MonitoringSystem() {

           @Override
           public void startMonitoring() {
               System.out.println("Мониторинг отслеживания ошибок стартовал!");
           }

           public int getCurrentErrorsCount() {

               return currentErrorsCount;
           }
       };
   }
}
```
Есть у них кое-что ==общее и с локальными классами==: ==они видны только внутри того метода, в котором определены.== В примере выше, любые попытки обратиться к объекту

```Plain
errorModule
```
   
за пределами метода

```Java
main()
```

будут неудачными.  
![[Pasted image 20240910141553.png]]
![[Pasted image 20240910141710.png]]
![[Pasted image 20240910141858.png]]


![[Pasted image 20240910142514.png]]
- Мы можем всё таки создать несколько объектов анонимного класса, но через рефликсию

![[Pasted image 20240910142459.png]]
![[Pasted image 20240910142658.png]]
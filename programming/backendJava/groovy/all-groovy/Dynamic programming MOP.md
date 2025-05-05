---
title: Dynamic programming MOP
tags:
  - Groovy
related_topics: 
created: 2024-09-17 18:18
modified: 2024-09-17T18:18:35+03:00
questions: 
notes: 
links: 
---

Dynamic programming. MOP

`_Groovy_` _- это_ `_JVM_` _язык, следовательно, он должен быть невероятно похож на_ `_Java_`_, т.к. компилируется в байт код. Тогда каким образом можно было изменить настолько природу языка_ `_Groovy_`_:_ ==_привнести динамическую составляющую_==_. Ведь_ `_Java_` _- это чисто_ ==_статический язык,_== _и после компиляции невозможно добавить/изменить функционал в классах. А все на самом деле просто -_ ==_добавлена специальная прослойка_== ==_MOP (Meta Object Protocol)_==_, и_ ==_все вызовы из_== `_Groovy_` ==_классов проксируются через вспомогательные классы_== ==_MOP_==_._

![[images/Untitled 52 2.png|Untitled 52 2.png]]

![[images/Untitled 53 2.png|Untitled 53 2.png]]

![[images/Untitled 54 2.png|Untitled 54 2.png]]

  
---
title: Настройка SSH keygen
tags:
  - Git
related_topics: 
created: 2024-09-16 18:37
modified: 2024-09-16T18:37:33+03:00
questions: 
notes: 
links: 
---

### Настройка SSH keygen

git config --global [user.name](http://user.name/) "Onemyname"  
git config --global user.email  
konvvadim@yandex.ru  
ssh-keygen -o -t rsa -C "  
konvvadim@yandex.ru"

cat /home/vadim/.ssh/id_rsa.pub  
Копируешь SSH вставляешь в GitHub-> Setting>SSH and keygens -> new device
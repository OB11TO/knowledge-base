---
title: Базовое описание Liquibase
tags:
  - Liquibase
related_topics: 
created: 2025-04-30 14:47
modified: 2025-04-30T16:09:55+03:00
questions: 
notes: 
links: 
---


------
[[Contexts и Labels Liquibase]]
[[Preconditions Liquibase]]


-----

Liquibase – это инструмент управления миграциями базы данных в стиле Database DevOps. <mark class="hltr-yellow">Он позволяет описывать изменения схемы БД как код и автоматически отслеживать, какие изменения уже применены</mark>​[liquibase.com](https://liquibase.com/how-liquibase-works#:~:text=By%20extending%20your%20CI%2FCD%20pipeline,have%20yet%20to%20go%20live)​[liquibase.com](https://liquibase.com/how-liquibase-works#:~:text=Built%20on%20top%20of%20its,improving%20security%2C%20governance%2C%20and%20compliance). В отличие от ручных скриптов, <mark class="hltr-green2">Liquibase хранит «книгу изменений» (changelog) в системе контроля версий и при каждом запуске выполняет только новые изменения, обеспечивая безопасное выполнение миграций и возможность отката</mark>​[liquibase.com](https://liquibase.com/how-liquibase-works#:~:text=By%20extending%20your%20CI%2FCD%20pipeline,have%20yet%20to%20go%20live)​[liquibase.com](https://liquibase.com/how-liquibase-works#:~:text=Built%20on%20top%20of%20its,improving%20security%2C%20governance%2C%20and%20compliance). Например, в документации Liquibase указывается, что SQL-операторы объявляются как _changesets_ внутри _changelog_-файла, после чего Liquibase применяет эти changesets к целевой БД​[docs.liquibase.com](https://docs.liquibase.com/start/get-started/liquibase-sql.html#:~:text=With%20Liquibase%2C%20SQL%20statements%20,3)​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/home.html#:~:text=With%20Liquibase%2C%20you%20use%20a,in%20any%20source%20control%20tool). Основные сущности Liquibase включают:

- **Changelog** – файл миграций (в формате SQL, XML, YAML или JSON), где последовательно описаны все изменения базы данных. В этом файле указываются все _changeset_-ы. Как отмечено в официальной документации, changelog – это «текстовый файл, в котором последовательно перечисляются все изменения, внесённые в вашу базу данных»​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/home.html#:~:text=With%20Liquibase%2C%20you%20use%20a,in%20any%20source%20control%20tool)​[docs.liquibase.com](https://docs.liquibase.com/start/get-started/liquibase-sql.html#:~:text=With%20Liquibase%2C%20SQL%20statements%20,3). Он хранится в Git или другой системе контроля версий и служит источником правды о состоянии схемы.
    
- **Changeset** – единица одного изменения (операция) в changelog. Каждый changeset имеет уникальную пару `id` и `author` и содержит SQL или соответствующие теги изменения. Например, один changeset может описывать создание таблицы, другой – изменение столбца. После применения changeset’а Liquibase помечает его как выполненный; повторно он не выполнится​[docs.liquibase.com](https://docs.liquibase.com/start/get-started/liquibase-sql.html#:~:text=With%20Liquibase%2C%20SQL%20statements%20,3)​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/home.html#:~:text=An%20individual%20unit%20of%20change,a%20changeset%20to%20create%20a). В документации указано: _«An individual unit of change in your changelog is called a **changeset**. Когда вы хотите изменить базу, достаточно добавить новый changeset с нужной операцией»_​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/home.html#:~:text=An%20individual%20unit%20of%20change,a%20changeset%20to%20create%20a)​[docs.liquibase.com](https://docs.liquibase.com/start/get-started/liquibase-sql.html#:~:text=With%20Liquibase%2C%20SQL%20statements%20,3).

-----------------------

- **Preconditions** – условия (preconditions) перед выполнением changeset’а. <mark class="hltr-green2">Они позволяют проверить состояние БД (например, наличие таблицы или версии СУБД) и в зависимости от этого контролировать запуск изменения. Если условие не выполняется, Liquibase может пропустить или остановить выполнение этого changeset’а</mark>. Официально это описано так: _«Preconditions – это теги в changelog или changeset, которые контролируют выполнение update на основе состояния БД. Если precondition для changeset’а не выполнено, Liquibase не применит этот changeset»_​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/preconditions.html#:~:text=Preconditions%20are%20tags%20you%20add,does%20not%20deploy%20that%20changeset).


--------------------
- **Contexts и Labels** – теги для группировки и фильтрации changeset’ов при запуске. Context (контекст) обычно используется для разделения окружений (например, `dev`, `test`, `prod`): к changeset’у добавляется атрибут `context`, и при запуске с флагом `--context=<value>` применяются только соответствующие changesets​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/attributes/contexts.html#:~:text=Contexts%20are%20tags%20that%20control,specify%20one%20or%20more%20changeset%C2%A0contexts). Labels (метки) – похожий механизм, но часто применяемый для группировки по функционалу или версии. Labels задаются через атрибут `labels` и фильтруются флагом `--label-filter`. Официальная документация описывает их так: _«Labels – теги, которые контролируют, будут ли выполняться определённые changeset’ы. Вы задаёте labels в changelog и фильтруете их с помощью `--label-filter` при запуске»_​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/attributes/labels.html#:~:text=Labels%20are%20tags%20that%20control,specify%20one%20or%20more%20changeset%C2%A0labels)​[docs.liquibase.com](https://docs.liquibase.com/concepts/changelogs/attributes/contexts.html#:~:text=Contexts%20are%20tags%20that%20control,specify%20one%20or%20more%20changeset%C2%A0contexts).

-----

#### **Роль Liquibase**

Liquibase — инструмент для управления изменениями схемы БД через _версионируемые миграции_.  
**Преимущества перед ручными скриптами**:

- **Автоматизация** отслеживания выполненных изменений (через таблицы `DATABASECHANGELOG`, `DATABASECHANGELOGLOCK`).
    
- **Кроссплатформенность**: абстракция над SQL (поддержка всех СУБД).
    
- **Atomic Rollbacks**: возможность отката изменений.
    
- **Интеграция с CI/CD**: запуск миграций в пайплайнах.

-----



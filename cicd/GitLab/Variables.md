---
created-dt: 2026-09-21 17:58
tags:
  - review
sr-due: 2026-09-29
sr-interval: 5
sr-ease: 248
---
Переменные в [[GitLab]] позволяют передавать данные в [[cicd/GitLab/Job|Job]]s без необходимости прописывать значения непосредственно в командах.

Например:
```
variables:
  APP_NAME: my-app

build:
  script:
    - echo "$APP_NAME"
```

Значение переменной будет доступно внутри job как переменная окружения.

### Глобальные и локальные переменные

Переменные можно определить для всего pipeline:
```
variables:
  APP_ENV: production

test:
  script:
    - echo "$APP_ENV"

deploy:
  script:
    - echo "$APP_ENV"
```

Или только для конкретной job:
```
test:
  variables:
    APP_ENV: testing

  script:
    - echo "$APP_ENV"
```

Локальная переменная с одинаковым именем переопределяет глобальную переменную из YAML.

### CI/CD Variables в интерфейсе GitLab

Переменные можно задавать через:

```
Settings
└── CI/CD
    └── Variables
```

Например:
```
Key:   SERVER_IP
Value: 192.168.1.100
```

Использование:
```
deploy:
  script:
    - ssh "user@$SERVER_IP"
```

Такие переменные не обязательно прописывать в `.gitlab-ci.yml`.

Их можно создавать на уровне проекта, группы или всего GitLab instance.

### Секреты

В GitLab нет отдельного механизма `secrets`, полностью аналогичного GitHub Actions Secrets. Чувствительные значения также можно хранить в CI/CD Variables.

Например:
```
SSH_PRIVATE_KEY
DB_PASSWORD
API_TOKEN
```

Для них доступны дополнительные настройки:

|Настройка|Назначение|
|---|---|
|Masked|Скрывает значение переменной в логах|
|Protected|Ограничивает доступ к переменной для protected branches/tags|
|Environment scope|Ограничивает доступ определённым environment|
|File|Передаёт содержимое через временный файл|

**Masked не гарантирует полной защиты секрета от утечки через вредоносный код job.** Поэтому важно ограничивать доступ к секретам и проверять изменения CI/CD-конфигурации.

### File variables

Полезны для передачи сертификатов, SSH-ключей и других файлов.

Например, через интерфейс GitLab создаётся переменная:
```
Key: SSH_PRIVATE_KEY
Type: File
```

GitLab Runner создаёт временный файл с её содержимым, а переменная содержит путь к нему.

Использование:
```
deploy:
  script:
    - chmod 600 "$SSH_PRIVATE_KEY"
    - ssh -i "$SSH_PRIVATE_KEY" user@server
```

### Predefined Variables

GitLab автоматически предоставляет набор встроенных переменных с информацией о текущем pipeline, job и проекте.

Например:

|Переменная|Значение|
|---|---|
|`$CI_COMMIT_BRANCH`|Текущая ветка, если переменная доступна для данного типа pipeline|
|`$CI_COMMIT_SHA`|SHA текущего коммита|
|`$CI_PIPELINE_SOURCE`|Источник создания pipeline|
|`$CI_PROJECT_NAME`|Название проекта|
|`$CI_JOB_ID`|ID текущей job|
|`$CI_JOB_STAGE`|Stage текущей job|

Пример:
```
info:
  script:
    - echo "$CI_PROJECT_NAME"
    - echo "$CI_COMMIT_SHA"
    - echo "$CI_PIPELINE_SOURCE"
```

Эти переменные не нужно создавать самостоятельно.

Они также используются в `rules`:
```
deploy:
  script:
    - ./deploy.sh

  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

Главная идея:
```
Variables
│
├── YAML variables
│   ├── Global
│   └── Job
│
├── UI Variables
│   ├── Project
│   ├── Group
│   └── Instance
│
└── Predefined Variables
    └── Создаются GitLab автоматически
```
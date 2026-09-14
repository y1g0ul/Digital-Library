---
created-dt: 2026-07-28 11:57
tags:
  - review
sr-due: 2026-10-28
sr-interval: 57
sr-ease: 254
---
Описание процесса автоматизации, который выполняется после наступления [[Event]], сценарий (план) выполнения [[cicd/GitHub Actions/Pipeline]].

Workflow отвечает на вопрос:
> *"Что нужно сделать после события?"*

Содержит:
- [[cicd/GitHub Actions/Job]]s
- условия запуска ([[Event]])
- переменные
- разрешения
- другие настройки

Event: `push`
↓
Workflow:
- Получить код из репозитория.
- Установить зависимости.
- Запустить тесты.
- Собрать [[docker]]-образ.
- Отправить образ в Registry.

GitHub Actions:
`.github/workflows/*.yml`

GitLab CI:
`.gitlab-ci.yml`

При необходимости можно отключить в интерфейсе сервиса, в описании [[commit]] указать `[actions skip]` или в самом [[cicd/GitHub Actions/YAML]] файле в [[Event]]s указать `workflow_dispatch` (ручной запуск).

Для запуска последовательно используется параметр `workflow_run` в [[Event]]. И так же в зависимости от результата работы прошлого Workflow могут меняться [[cicd/GitHub Actions/Job]]s.
``` yaml
on:
  workflow_run: # Allows you to run this workflow manually from the Actions tab
    workflows: [First Workflow]
    types:
      - completed
        
jobs:
  success-command:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    steps:
      run: echo "The Third Workflow has completed successfully!"

  failure-command:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'failure' }}
    steps:
      run: echo "The Third Workflow has failed."
```



